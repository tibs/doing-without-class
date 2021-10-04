===================
Doing without class
===================

Written as a talk for CamPUG_

History
~~~~~~~

The talk was (will be) given at CamPUG_ on Tuesday 5th October 2021.

.. _CamPUG: https://www.meetup.com/CamPUG/events/280947413/

Running the notebook
~~~~~~~~~~~~~~~~~~~~

This requires Python3 and `jupyter notebook`_

Clone this repository, and ``cd`` into its directory.

If you have poetry_, then set up the environment and dependencies as follows::

  $ poetry shell
  $ poetry install

Run the notebook with::

  $ jupyter notebook doing-without-class.ipynb

.. _poetry: https://python-poetry.org/
.. _`jupyter notebook`: https://jupyter.readthedocs.io/en/latest/running.html#running

I also like to enable vim bindings, using https://github.com/lambdalisue/jupyter-vim-binding

If you have installed RISE_ (which is in the poetry setup) then you can run
the notebook as a slideshow - there should be an "Enter/Exit RISE slideshow"
button in the notebook toolbar (it's a mini-picture of a histogram).

.. _RISE: https://rise.readthedocs.io/en/stable/

**Note** that running the notebook changes its content (the results of all
those code cells). If you haven't edited the notebook, these can be ignored.
Otherwise, use ``git diff -u doing-without-class.ipynb`` to see if
the changes are significant or not.

The HTML and PDF versions
~~~~~~~~~~~~~~~~~~~~~~~~~

I've also saved the notebook as:

* `HTML slides`_ - you'll need to download this to use it
* `HTML`_ - you'll need to download this to use it
* markdown_ - this can be viewed in github
* `PDF`_, although it's missing the `flowchart image`_ from
  https://terribleminds.com/ramble/2013/08/06/are-you-a-real-writer-a-handy-and-hasty-flowchart/

Since github won't serve HTML files, you will have to clone the repository
locally to be able to display the HTML files (or download the file and its
dependencies).

.. _`HTML slides`: doing-without-class.slides.html
.. _`markdown`: doing-without-class.md
.. _`HTML`: doing-without-class.html
.. _`PDF`: doing-without-class.pdf
.. _`flowchart image`: am-i-a-writer.webp

--------

  |cc-attr-sharealike|

  This Jupyter notebook and its related files are released under a `Creative Commons
  Attribution-ShareAlike 4.0 International License`_.
  **except** for the flowchart image from
  https://terribleminds.com/ramble/2013/08/06/are-you-a-real-writer-a-handy-and-hasty-flowchart/
  which belongs to Chuck Wendig.

.. |cc-attr-sharealike| image:: images/cc-attribution-sharealike-88x31.png
   :alt: CC-Attribution-ShareAlike image

.. _`Creative Commons Attribution-ShareAlike 4.0 International License`: http://creativecommons.org/licenses/by-sa/4.0/
