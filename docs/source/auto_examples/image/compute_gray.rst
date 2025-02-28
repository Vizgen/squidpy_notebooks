
.. DO NOT EDIT.
.. THIS FILE WAS AUTOMATICALLY GENERATED BY SPHINX-GALLERY.
.. TO MAKE CHANGES, EDIT THE SOURCE PYTHON FILE:
.. "auto_examples/image/compute_gray.py"
.. LINE NUMBERS ARE GIVEN BELOW.

.. only:: html

  .. container:: binder-badge

    .. image:: images/binder_badge_logo.svg
      :target: https://mybinder.org/v2/gh/scverse/squidpy_notebooks/main?filepath=docs/source/auto_examples/image/compute_gray.ipynb
      :alt: Launch binder
      :width: 150 px

.. rst-class:: sphx-glr-example-title

.. _sphx_glr_auto_examples_image_compute_gray.py:

Convert to grayscale
--------------------

This example shows how to use :func:`squidpy.im.process` to convert an image layer to grayscale.

You can convert any layer of :class:`squidpy.im.ImageContainer` to grayscale.
We use the argument ``method = 'gray'`` to convert the image.
This calls :func:`skimage.color.rgb2gray` in the background.

.. seealso::

    - :ref:`sphx_glr_auto_examples_image_compute_smooth.py`
    - :ref:`sphx_glr_auto_examples_image_compute_process_hires.py`

.. GENERATED FROM PYTHON SOURCE LINES 17-22

.. code-block:: default


    import squidpy as sq

    import matplotlib.pyplot as plt








.. GENERATED FROM PYTHON SOURCE LINES 23-27

First, we load the H&E stained tissue image.
Here, we only load a cropped dataset to speed things up.
In general, :func:`squidpy.im.process` can also process very large images
(see :ref:`sphx_glr_auto_examples_image_compute_process_hires.py`).

.. GENERATED FROM PYTHON SOURCE LINES 27-29

.. code-block:: default

    img = sq.datasets.visium_hne_image_crop()








.. GENERATED FROM PYTHON SOURCE LINES 30-37

Then, we convert the image to grayscale and plot the result.
With the argument ``layer`` we can select the image layer that should be processed.
When converting to grayscale, the channel dimensions change from 3 to 1.
By default, the name of the resulting channel dimension will be ``'{{original_channel_name}}_gray'``.
Use the argument ``channel_dim`` to set a new channel name explicitly.
By default, the resulting image is saved in the layer ``image_gray``.
This behavior can be changed with the arguments ``copy`` and ``layer_added``.

.. GENERATED FROM PYTHON SOURCE LINES 37-44

.. code-block:: default

    sq.im.process(img, layer="image", method="gray")

    fig, axes = plt.subplots(1, 2)
    img.show("image", ax=axes[0])
    _ = axes[0].set_title("original")
    img.show("image_gray", cmap="gray", ax=axes[1])
    _ = axes[1].set_title("grayscale")



.. image-sg:: /auto_examples/image/images/sphx_glr_compute_gray_001.png
   :alt: original, grayscale
   :srcset: /auto_examples/image/images/sphx_glr_compute_gray_001.png
   :class: sphx-glr-single-img






.. rst-class:: sphx-glr-timing

   **Total running time of the script:** ( 0 minutes  9.123 seconds)

**Estimated memory usage:**  707 MB


.. _sphx_glr_download_auto_examples_image_compute_gray.py:


.. only :: html

 .. container:: sphx-glr-footer
    :class: sphx-glr-footer-example



  .. container:: sphx-glr-download sphx-glr-download-python

     :download:`Download Python source code: compute_gray.py <compute_gray.py>`



  .. container:: sphx-glr-download sphx-glr-download-jupyter

     :download:`Download Jupyter notebook: compute_gray.ipynb <compute_gray.ipynb>`
