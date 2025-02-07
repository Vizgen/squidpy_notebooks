
.. DO NOT EDIT.
.. THIS FILE WAS AUTOMATICALLY GENERATED BY SPHINX-GALLERY.
.. TO MAKE CHANGES, EDIT THE SOURCE PYTHON FILE:
.. "auto_tutorials/tutorial_seqfish.py"
.. LINE NUMBERS ARE GIVEN BELOW.

.. only:: html

  .. container:: binder-badge

    .. image:: images/binder_badge_logo.svg
      :target: https://mybinder.org/v2/gh/scverse/squidpy_notebooks/main?filepath=docs/source/auto_tutorials/tutorial_seqfish.ipynb
      :alt: Launch binder
      :width: 150 px

.. rst-class:: sphx-glr-example-title

.. _sphx_glr_auto_tutorials_tutorial_seqfish.py:

Analyze seqFISH data
====================

This tutorial shows how to apply Squidpy for the analysis of seqFISH data.

The data used here was obtained from :cite:`lohoff2020highly`.
We provide a pre-processed subset of the data, in :class:`anndata.AnnData` format.
For details on how it was pre-processed, please refer to the original paper.

.. seealso::

    See :ref:`sphx_glr_auto_tutorials_tutorial_imc.py` for additional analysis examples.

Import packages & data
----------------------
To run the notebook locally, create a conda environment as *conda env create -f environment.yml* using this
`environment.yml <https://github.com/scverse/squidpy_notebooks/blob/main/environment.yml>`_.

.. GENERATED FROM PYTHON SOURCE LINES 21-33

.. code-block:: default


    import scanpy as sc
    import squidpy as sq

    import numpy as np

    sc.logging.print_header()
    print(f"squidpy=={sq.__version__}")

    # load the pre-processed dataset
    adata = sq.datasets.seqfish()





.. rst-class:: sphx-glr-script-out

 Out:

 .. code-block:: none

    scanpy==1.9.1 anndata==0.8.0 umap==0.5.3 numpy==1.22.4 scipy==1.8.1 pandas==1.4.2 scikit-learn==1.1.1 statsmodels==0.13.2 python-igraph==0.9.11 pynndescent==0.5.7
    squidpy==1.2.2




.. GENERATED FROM PYTHON SOURCE LINES 34-36

First, let's visualize cluster annotation in spatial context
with :func:`squidpy.pl.spatial_scatter`.

.. GENERATED FROM PYTHON SOURCE LINES 36-38

.. code-block:: default

    sq.pl.spatial_scatter(adata, color="celltype_mapped_refined", shape=None, figsize=(10, 10))




.. image-sg:: /auto_tutorials/images/sphx_glr_tutorial_seqfish_001.png
   :alt: celltype_mapped_refined
   :srcset: /auto_tutorials/images/sphx_glr_tutorial_seqfish_001.png
   :class: sphx-glr-single-img





.. GENERATED FROM PYTHON SOURCE LINES 39-59

Neighborhood enrichment analysis
--------------------------------
Similar to other spatial data, we can investigate spatial organization of clusters
in a quantitative way, by computing a neighborhood enrichment score.
You can compute such score with the following function: :func:`squidpy.gr.nhood_enrichment`.
In short, it's an enrichment score on spatial proximity of clusters:
if spots belonging to two different clusters are often close to each other,
then they will have a high score and can be defined as being *enriched*.
On the other hand, if they are far apart, the score will be low
and they can be defined as *depleted*.
This score is based on a permutation-based test, and you can set
the number of permutations with the `n_perms` argument (default is 1000).

Since the function works on a connectivity matrix, we need to compute that as well.
This can be done with :func:`squidpy.gr.spatial_neighbors`.
Please see :ref:`sphx_glr_auto_examples_graph_compute_spatial_neighbors.py` for more details
of how this function works.

Finally, we'll directly visualize the results with :func:`squidpy.pl.nhood_enrichment`.
We'll add a dendrogram to the heatmap computed with linkage method *ward*.

.. GENERATED FROM PYTHON SOURCE LINES 59-63

.. code-block:: default

    sq.gr.spatial_neighbors(adata, coord_type="generic")
    sq.gr.nhood_enrichment(adata, cluster_key="celltype_mapped_refined")
    sq.pl.nhood_enrichment(adata, cluster_key="celltype_mapped_refined", method="ward")




.. image-sg:: /auto_tutorials/images/sphx_glr_tutorial_seqfish_002.png
   :alt: Neighborhood enrichment
   :srcset: /auto_tutorials/images/sphx_glr_tutorial_seqfish_002.png
   :class: sphx-glr-single-img


.. rst-class:: sphx-glr-script-out

 Out:

 .. code-block:: none

      0%|          | 0/1000 [00:00<?, ?/s]      0%|          | 1/1000 [00:05<1:29:18,  5.36s/]      8%|8         | 80/1000 [00:05<00:44, 20.67/s]      16%|#6        | 160/1000 [00:05<00:17, 48.51/s]     24%|##4       | 241/1000 [00:05<00:08, 85.48/s]     32%|###2      | 321/1000 [00:05<00:05, 131.78/s]     40%|####      | 402/1000 [00:05<00:03, 189.17/s]     48%|####8     | 483/1000 [00:05<00:02, 256.00/s]     56%|#####6    | 563/1000 [00:06<00:01, 328.24/s]     64%|######4   | 644/1000 [00:06<00:00, 404.51/s]     72%|#######2  | 725/1000 [00:06<00:00, 479.70/s]     81%|########  | 806/1000 [00:06<00:00, 548.75/s]     89%|########8 | 887/1000 [00:06<00:00, 607.99/s]     97%|#########6| 968/1000 [00:06<00:00, 657.10/s]    100%|##########| 1000/1000 [00:06<00:00, 151.29/s]




.. GENERATED FROM PYTHON SOURCE LINES 64-80

A similar analysis was performed in the original publication :cite:`lohoff2020highly`,
and we can appreciate to what extent results overlap.
For instance, there seems to be an enrichment between the *Lateral plate mesoderm*,
the *Intermediate mesoderm* and a milder enrichment for *Allantois* cells.
As in the original publication, there also seems to be an association between the *Endothelium* and
the *Haematoendothelial progenitors*.
Of course, results do not perfectly overlap, and this could be due to several factors:

  - the construction of the neighbors graph (which in our case is
    not informed by the radius, as we did not have access to this information).
  - the number of permutation of the neighborhood enrichment
    (500 in the original publication against the default 1000 in our implementation).

We can also visualize the spatial organization of cells again,
and appreciate the proximity of specific cell clusters.
For this, we'll use :func:`squidpy.pl.spatial_scatter` again.

.. GENERATED FROM PYTHON SOURCE LINES 80-95

.. code-block:: default

    sq.pl.spatial_scatter(
        adata,
        color="celltype_mapped_refined",
        groups=[
            "Endothelium",
            "Haematoendothelial progenitors",
            "Allantois",
            "Lateral plate mesoderm",
            "Intermediate mesoderm",
            "Presomitic mesoderm",
        ],
        shape=None,
        size=2,
    )




.. image-sg:: /auto_tutorials/images/sphx_glr_tutorial_seqfish_003.png
   :alt: celltype_mapped_refined
   :srcset: /auto_tutorials/images/sphx_glr_tutorial_seqfish_003.png
   :class: sphx-glr-single-img





.. GENERATED FROM PYTHON SOURCE LINES 96-119

Co-occurrence across spatial dimensions
---------------------------------------
In addition to the neighbor enrichment score, we can visualize cluster co-occurrence
in spatial dimensions.
This is a similar analysis of the one presented above,
yet it does not operate on the connectivity matrix,
but on the original spatial coordinates.
The co-occurrence score is defined as:

.. math::

    \frac{p(exp|cond)}{p(exp)}

where :math:`p(exp|cond)` is the conditional probability of observing a
cluster :math:`exp` conditioned on the presence of a cluster :math:`cond`, whereas
:math:`p(exp)` is the probability of observing :math:`exp` in the radius size
of interest. The score is computed across increasing radii size
around each cell in the tissue.

We can compute this score with :func:`squidpy.gr.co_occurrence`
and set the cluster annotation for the conditional probability with
the argument ``clusters``. Then, we visualize the results with
:func:`squidpy.pl.co_occurrence`.

.. GENERATED FROM PYTHON SOURCE LINES 119-127

.. code-block:: default

    sq.gr.co_occurrence(adata, cluster_key="celltype_mapped_refined")
    sq.pl.co_occurrence(
        adata,
        cluster_key="celltype_mapped_refined",
        clusters="Lateral plate mesoderm",
        figsize=(10, 5),
    )




.. image-sg:: /auto_tutorials/images/sphx_glr_tutorial_seqfish_004.png
   :alt: $\frac{p(exp|Lateral plate mesoderm)}{p(exp)}$
   :srcset: /auto_tutorials/images/sphx_glr_tutorial_seqfish_004.png
   :class: sphx-glr-single-img


.. rst-class:: sphx-glr-script-out

 Out:

 .. code-block:: none

      0%|          | 0/1 [00:00<?, ?/s]    100%|##########| 1/1 [00:54<00:00, 54.61s/]    100%|##########| 1/1 [00:54<00:00, 54.73s/]
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1582: UserWarning: Trying to register the cmap 'rocket' which already exists.
      mpl_cm.register_cmap(_name, _cmap)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1583: UserWarning: Trying to register the cmap 'rocket_r' which already exists.
      mpl_cm.register_cmap(_name + "_r", _cmap_r)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1582: UserWarning: Trying to register the cmap 'mako' which already exists.
      mpl_cm.register_cmap(_name, _cmap)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1583: UserWarning: Trying to register the cmap 'mako_r' which already exists.
      mpl_cm.register_cmap(_name + "_r", _cmap_r)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1582: UserWarning: Trying to register the cmap 'icefire' which already exists.
      mpl_cm.register_cmap(_name, _cmap)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1583: UserWarning: Trying to register the cmap 'icefire_r' which already exists.
      mpl_cm.register_cmap(_name + "_r", _cmap_r)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1582: UserWarning: Trying to register the cmap 'vlag' which already exists.
      mpl_cm.register_cmap(_name, _cmap)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1583: UserWarning: Trying to register the cmap 'vlag_r' which already exists.
      mpl_cm.register_cmap(_name + "_r", _cmap_r)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1582: UserWarning: Trying to register the cmap 'flare' which already exists.
      mpl_cm.register_cmap(_name, _cmap)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1583: UserWarning: Trying to register the cmap 'flare_r' which already exists.
      mpl_cm.register_cmap(_name + "_r", _cmap_r)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1582: UserWarning: Trying to register the cmap 'crest' which already exists.
      mpl_cm.register_cmap(_name, _cmap)
    /Users/giovanni.palla/Projects/squidpy_notebooks/.tox/docs/lib/python3.9/site-packages/seaborn/cm.py:1583: UserWarning: Trying to register the cmap 'crest_r' which already exists.
      mpl_cm.register_cmap(_name + "_r", _cmap_r)




.. GENERATED FROM PYTHON SOURCE LINES 128-137

It seems to recapitulate a previous observation, that there is a co-occurrence between the
conditional cell type annotation *Lateral plate mesoderm* and the clusters
*Intermediate mesoderm* and *Allantois*.
It also seems that at longer distances, there is a co-occurrence of cells belonging to
the *Presomitic mesoderm* cluster. By visualizing the full tissue as before we can indeed
appreciate that these cell types seems to form a defined clusters relatively close
to the *Lateral plate mesoderm* cells.
It should be noted that the distance units corresponds to
the spatial coordinates saved in `adata.obsm['spatial']`.

.. GENERATED FROM PYTHON SOURCE LINES 139-159

Ligand-receptor interaction analysis
------------------------------------
The analysis showed above has provided us with quantitative information on
cellular organization and communication at the tissue level.
We might be interested in getting a list of potential candidates that might be driving
such cellular communication.
This naturally translates in doing a ligand-receptor interaction analysis.
In Squidpy, we provide a fast re-implementation the popular method CellPhoneDB :cite:`cellphonedb`
(`code <https://github.com/Teichlab/cellphonedb>`_)
and extended its database of annotated ligand-receptor interaction pairs with
the popular database *Omnipath* :cite:`omnipath`.
You can run the analysis for all clusters pairs, and all genes (in seconds,
without leaving this notebook), with :func:`squidpy.gr.ligrec`.

Let's perform the analysis and visualize the result for three clusters of
interest: *Lateral plate mesoderm*,
*Intermediate mesoderm* and *Allantois*. For the visualization, we will
filter out annotations
with low-expressed genes (with the ``means_range`` argument)
and decreasing the threshold for the adjusted p-value (with the ``alpha`` argument).

.. GENERATED FROM PYTHON SOURCE LINES 159-174

.. code-block:: default

    sq.gr.ligrec(
        adata,
        n_perms=100,
        cluster_key="celltype_mapped_refined",
    )
    sq.pl.ligrec(
        adata,
        cluster_key="celltype_mapped_refined",
        source_groups="Lateral plate mesoderm",
        target_groups=["Intermediate mesoderm", "Allantois"],
        means_range=(0.3, np.inf),
        alpha=1e-4,
        swap_axes=True,
    )




.. image-sg:: /auto_tutorials/images/sphx_glr_tutorial_seqfish_005.png
   :alt: Receptor-ligand test, $-\log_{10} ~ P$, significant $p=0.0001$, $log_2(\frac{molecule_1 + molecule_2}{2} + 1)$
   :srcset: /auto_tutorials/images/sphx_glr_tutorial_seqfish_005.png
   :class: sphx-glr-single-img


.. rst-class:: sphx-glr-script-out

 Out:

 .. code-block:: none

      0%|          | 0/100 [00:00<?, ?permutation/s]      1%|1         | 1/100 [00:04<07:27,  4.52s/permutation]     63%|######3   | 63/100 [00:04<00:01, 19.21permutation/s]    100%|##########| 100/100 [00:04<00:00, 21.39permutation/s]




.. GENERATED FROM PYTHON SOURCE LINES 175-180

The dotplot visualization provides an interesting set of candidate interactions
that could be involved in the tissue organization of the cell types of interest.
It should be noted that this method is a pure re-implementation of the original
permutation-based test, and therefore retains all its caveats
and should be interpreted accordingly.


.. rst-class:: sphx-glr-timing

   **Total running time of the script:** ( 1 minutes  48.024 seconds)

**Estimated memory usage:**  2512 MB


.. _sphx_glr_download_auto_tutorials_tutorial_seqfish.py:


.. only :: html

 .. container:: sphx-glr-footer
    :class: sphx-glr-footer-example



  .. container:: sphx-glr-download sphx-glr-download-python

     :download:`Download Python source code: tutorial_seqfish.py <tutorial_seqfish.py>`



  .. container:: sphx-glr-download sphx-glr-download-jupyter

     :download:`Download Jupyter notebook: tutorial_seqfish.ipynb <tutorial_seqfish.ipynb>`
