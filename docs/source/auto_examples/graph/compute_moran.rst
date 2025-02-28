
.. DO NOT EDIT.
.. THIS FILE WAS AUTOMATICALLY GENERATED BY SPHINX-GALLERY.
.. TO MAKE CHANGES, EDIT THE SOURCE PYTHON FILE:
.. "auto_examples/graph/compute_moran.py"
.. LINE NUMBERS ARE GIVEN BELOW.

.. only:: html

  .. container:: binder-badge

    .. image:: images/binder_badge_logo.svg
      :target: https://mybinder.org/v2/gh/scverse/squidpy_notebooks/main?filepath=docs/source/auto_examples/graph/compute_moran.ipynb
      :alt: Launch binder
      :width: 150 px

.. rst-class:: sphx-glr-example-title

.. _sphx_glr_auto_examples_graph_compute_moran.py:

Compute Moran's I score
-----------------------

This example shows how to compute the Moran's I global spatial auto-correlation statistics.

The Moran's I global spatial auto-correlation statistics evaluates whether
features (i.e. genes) shows a pattern that is clustered, dispersed or random
in the tissue are under consideration.

.. seealso::

    - See :ref:`sphx_glr_auto_examples_graph_compute_co_occurrence.py` and
      :ref:`sphx_glr_auto_examples_graph_compute_ripley.py` for other scores to describe spatial patterns.
    - See :ref:`sphx_glr_auto_examples_graph_compute_spatial_neighbors.py` for general usage of
      :func:`squidpy.gr.spatial_neighbors`.

.. GENERATED FROM PYTHON SOURCE LINES 19-24

.. code-block:: default

    import squidpy as sq

    adata = sq.datasets.visium_hne_adata()
    adata





.. rst-class:: sphx-glr-script-out

 Out:

 .. code-block:: none


    AnnData object with n_obs × n_vars = 2688 × 18078
        obs: 'in_tissue', 'array_row', 'array_col', 'n_genes_by_counts', 'log1p_n_genes_by_counts', 'total_counts', 'log1p_total_counts', 'pct_counts_in_top_50_genes', 'pct_counts_in_top_100_genes', 'pct_counts_in_top_200_genes', 'pct_counts_in_top_500_genes', 'total_counts_mt', 'log1p_total_counts_mt', 'pct_counts_mt', 'n_counts', 'leiden', 'cluster'
        var: 'gene_ids', 'feature_types', 'genome', 'mt', 'n_cells_by_counts', 'mean_counts', 'log1p_mean_counts', 'pct_dropout_by_counts', 'total_counts', 'log1p_total_counts', 'n_cells', 'highly_variable', 'highly_variable_rank', 'means', 'variances', 'variances_norm'
        uns: 'cluster_colors', 'hvg', 'leiden', 'leiden_colors', 'neighbors', 'pca', 'rank_genes_groups', 'spatial', 'umap'
        obsm: 'X_pca', 'X_umap', 'spatial'
        varm: 'PCs'
        obsp: 'connectivities', 'distances'



.. GENERATED FROM PYTHON SOURCE LINES 25-28

We can compute the Moran's I score with :func:`squidpy.gr.spatial_autocorr` and ``mode = 'moran'``.
We first need to compute a spatial graph with :func:`squidpy.gr.spatial_neighbors`.
We will also subset the number of genes to evaluate.

.. GENERATED FROM PYTHON SOURCE LINES 28-39

.. code-block:: default

    genes = adata[:, adata.var.highly_variable].var_names.values[:100]
    sq.gr.spatial_neighbors(adata)
    sq.gr.spatial_autocorr(
        adata,
        mode="moran",
        genes=genes,
        n_perms=100,
        n_jobs=1,
    )
    adata.uns["moranI"].head(10)





.. rst-class:: sphx-glr-script-out

 Out:

 .. code-block:: none

      0%|          | 0/100 [00:00<?, ?/s]      1%|1         | 1/100 [00:05<09:10,  5.56s/]      3%|3         | 3/100 [00:05<02:25,  1.50s/]      5%|5         | 5/100 [00:05<01:13,  1.29/s]      7%|7         | 7/100 [00:06<00:44,  2.08/s]      9%|9         | 9/100 [00:06<00:30,  3.02/s]     11%|#1        | 11/100 [00:06<00:21,  4.10/s]     13%|#3        | 13/100 [00:06<00:16,  5.28/s]     15%|#5        | 15/100 [00:06<00:13,  6.46/s]     17%|#7        | 17/100 [00:06<00:11,  7.54/s]     19%|#9        | 19/100 [00:07<00:09,  8.52/s]     21%|##1       | 21/100 [00:07<00:08,  9.33/s]     23%|##3       | 23/100 [00:07<00:07, 10.07/s]     25%|##5       | 25/100 [00:07<00:07, 10.59/s]     27%|##7       | 27/100 [00:07<00:06, 11.09/s]     29%|##9       | 29/100 [00:07<00:06, 11.40/s]     31%|###1      | 31/100 [00:08<00:05, 11.59/s]     33%|###3      | 33/100 [00:08<00:05, 11.83/s]     35%|###5      | 35/100 [00:08<00:05, 11.83/s]     37%|###7      | 37/100 [00:08<00:05, 11.94/s]     39%|###9      | 39/100 [00:08<00:05, 12.08/s]     41%|####1     | 41/100 [00:08<00:04, 12.15/s]     43%|####3     | 43/100 [00:09<00:04, 12.17/s]     45%|####5     | 45/100 [00:09<00:04, 12.27/s]     47%|####6     | 47/100 [00:09<00:04, 12.29/s]     49%|####9     | 49/100 [00:09<00:04, 12.33/s]     51%|#####1    | 51/100 [00:09<00:03, 12.37/s]     53%|#####3    | 53/100 [00:09<00:03, 12.37/s]     55%|#####5    | 55/100 [00:09<00:03, 12.32/s]     57%|#####6    | 57/100 [00:10<00:03, 12.30/s]     59%|#####8    | 59/100 [00:10<00:03, 12.27/s]     61%|######1   | 61/100 [00:10<00:03, 12.30/s]     63%|######3   | 63/100 [00:10<00:03, 12.30/s]     65%|######5   | 65/100 [00:10<00:02, 12.25/s]     67%|######7   | 67/100 [00:10<00:02, 12.28/s]     69%|######9   | 69/100 [00:11<00:02, 12.34/s]     71%|#######1  | 71/100 [00:11<00:02, 12.35/s]     73%|#######3  | 73/100 [00:11<00:02, 12.35/s]     75%|#######5  | 75/100 [00:11<00:02, 12.30/s]     77%|#######7  | 77/100 [00:11<00:01, 12.38/s]     79%|#######9  | 79/100 [00:11<00:01, 12.35/s]     81%|########1 | 81/100 [00:12<00:01, 12.37/s]     83%|########2 | 83/100 [00:12<00:01, 12.38/s]     85%|########5 | 85/100 [00:12<00:01, 12.41/s]     87%|########7 | 87/100 [00:12<00:01, 12.42/s]     89%|########9 | 89/100 [00:12<00:00, 12.44/s]     91%|#########1| 91/100 [00:12<00:00, 12.38/s]     93%|#########3| 93/100 [00:13<00:00, 12.39/s]     95%|#########5| 95/100 [00:13<00:00, 12.44/s]     97%|#########7| 97/100 [00:13<00:00, 12.45/s]     99%|#########9| 99/100 [00:13<00:00, 12.44/s]    100%|##########| 100/100 [00:13<00:00,  7.34/s]


.. raw:: html

    <div class="output_subarea output_html rendered_html output_result">
    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }

        .dataframe tbody tr th {
            vertical-align: top;
        }

        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>I</th>
          <th>pval_norm</th>
          <th>var_norm</th>
          <th>pval_z_sim</th>
          <th>pval_sim</th>
          <th>var_sim</th>
          <th>pval_norm_fdr_bh</th>
          <th>pval_z_sim_fdr_bh</th>
          <th>pval_sim_fdr_bh</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>3110035E14Rik</th>
          <td>0.665132</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000333</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>Resp18</th>
          <td>0.649582</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000297</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>1500015O10Rik</th>
          <td>0.605940</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000250</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>Ecel1</th>
          <td>0.570304</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000310</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>2010300C02Rik</th>
          <td>0.539469</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000261</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>Scg2</th>
          <td>0.476060</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000252</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>Ogfrl1</th>
          <td>0.457945</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000183</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>Itm2c</th>
          <td>0.451842</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000186</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>Tuba4a</th>
          <td>0.451810</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000161</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
        <tr>
          <th>Satb2</th>
          <td>0.429162</td>
          <td>0.0</td>
          <td>0.000131</td>
          <td>0.0</td>
          <td>0.009901</td>
          <td>0.000212</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>0.011929</td>
        </tr>
      </tbody>
    </table>
    </div>
    </div>
    <br />
    <br />

.. GENERATED FROM PYTHON SOURCE LINES 40-41

We can visualize some of those genes with :func:`squidpy.pl.spatial_scatter`.

.. GENERATED FROM PYTHON SOURCE LINES 41-43

.. code-block:: default

    sq.pl.spatial_scatter(adata, color=["Resp18", "Tuba4a"])




.. image-sg:: /auto_examples/graph/images/sphx_glr_compute_moran_001.png
   :alt: Resp18, Tuba4a
   :srcset: /auto_examples/graph/images/sphx_glr_compute_moran_001.png
   :class: sphx-glr-single-img





.. GENERATED FROM PYTHON SOURCE LINES 44-46

We could've also passed ``mode = 'geary'`` to compute a closely related auto-correlation statistic, `Geary's C
<https://en.wikipedia.org/wiki/Geary%27s_C>`_. See :func:`squidpy.gr.spatial_autocorr` for more information.


.. rst-class:: sphx-glr-timing

   **Total running time of the script:** ( 0 minutes  28.803 seconds)

**Estimated memory usage:**  298 MB


.. _sphx_glr_download_auto_examples_graph_compute_moran.py:


.. only :: html

 .. container:: sphx-glr-footer
    :class: sphx-glr-footer-example



  .. container:: sphx-glr-download sphx-glr-download-python

     :download:`Download Python source code: compute_moran.py <compute_moran.py>`



  .. container:: sphx-glr-download sphx-glr-download-jupyter

     :download:`Download Jupyter notebook: compute_moran.ipynb <compute_moran.ipynb>`
