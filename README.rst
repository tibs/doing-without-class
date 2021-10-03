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

You can install `Jupyter notebook**_ and any other dependencies using poetry_::

  $ poetry install

Run the notebook with::

  $ poetry shell
  $ jupyter notebook doing-without-class.ipynb

.. _poetry: https://python-poetry.org/
.. _`jupyter notebook`: https://jupyter.readthedocs.io/en/latest/running.html#running

I also like to enable vim bindings, using https://github.com/lambdalisue/jupyter-vim-binding

**Note** that running the notebook changes its content (the results of all
those code cells). If you haven't edited the notebook, these can be ignored.
Otherwise, use ``git diff -u doing-without-class.ipynb`` to see if
the changes are significant or not.

The HTML and PDF versions
~~~~~~~~~~~~~~~~~~~~~~~~~

I've also saved the notebook as:

* `HTML slides`_
* `HTML`
* `PDF`_, although it's missing the `flowchart image`_

Since github won't serve HTML files, you will have to clone the repository
locally to be able to display the HTML files.

.. _`HTML slides`: doing-without-class.slides.html
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
