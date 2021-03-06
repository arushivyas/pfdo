pfdo
==================

.. image:: https://badge.fury.io/py/pfdo.svg
    :target: https://badge.fury.io/py/pfdo

.. image:: https://travis-ci.org/FNNDSC/pfdo.svg?branch=master
    :target: https://travis-ci.org/FNNDSC/pfdo

.. image:: https://img.shields.io/badge/python-3.5%2B-blue.svg
    :target: https://badge.fury.io/py/pfdo

.. contents:: Table of Contents


Quick Overview
--------------

-  ``pfdo`` demonstrates how to use ``pftree`` to transverse directory trees and execute a specific analysis at each directory level (that optionally contains files of interest).

Overview
--------

``pfdo`` is a reference / base class application that is typically used as a component for constructing more complex behavioured functions. The application leverages the ``pfree`` callback coding contract to target a specific directory with specific files in an arbitrary file tree.

For example, imagine a nested tree of JPG image files and imagine some application that processes JPGs (rotates, increases size, etc). Using a suitably sub-classed ``pfdo`` (for example pfdo_imgconvert), a developer is able to apply some necessary processing to the files of interest irrespective of where in some input tree structure the files exist.

Moreover, the results of the processing are stored in an output directory, in an output tree, that reflects the topology of the input tree.


Installation
------------

Dependencies
~~~~~~~~~~~~

The following dependencies are installed on your host system/python3 virtual env (they will also be automatically installed if pulled from pypi):

-  ``pfmisc`` (various misc modules and classes for the pf* family of objects)
-  ``pftree`` (create a dictionary representation of a filesystem hierarchy)

Using ``PyPI``
~~~~~~~~~~~~~~

The best method of installing this script and all of its dependencies is
by fetching it from PyPI

.. code:: bash

        pip3 install pfdo

Command line arguments
----------------------

.. code:: html


        -I|--inputDir <inputDir>
        Input base directory to traverse.

        -O|--outputDir <outputDir>
        The output root directory that will contain a tree structure identical
        to the input directory, and each "leaf" node will contain the analysis
        results.

        [-i|--inputFile <inputFile>]
        An optional <inputFile> specified relative to the <inputDir>. If
        specified, then do not perform a directory walk, but convert only
        this file.

        [-f|--filterExpression <someFilter>]
        An optional string to filter the files of interest from the
        <inputDir> tree.

        [--outputLeafDir <outputLeafDirFormat>]
        If specified, will apply the <outputLeafDirFormat> to the output
        directories containing data. This is useful to blanket describe
        final output directories with some descriptive text, such as
        'anon' or 'preview'.

        This is a formatting spec, so

            --outputLeafDir 'preview-%%s'

        where %%s is the original leaf directory node, will prefix each
        final directory containing output with the text 'preview-' which
        can be useful in describing some features of the output set.

        [--test]
        If specified, run the "dummy" internal callback loop triad. The test
        flow simply tags files in some inputDir tree and "touches" them to a
        reconstiuted tree in the output directory, prefixed with the text
        "analyzed-".

        [--threads <numThreads>]
        If specified, break the innermost analysis loop into <numThreads>
        threads.

        [-x|--man]
        Show full help.

        [-y|--synopsis]
        Show brief help.

        [--json]
        If specified, output a JSON dump of final return.

        [--followLinks]
        If specified, follow symbolic links.

        -v|--verbosity <level>
        Set the app verbosity level.

            0: No internal output;
            1: Run start / stop output notification;
            2: As with level '1' but with simpleProgress bar in 'pftree';
            3: As with level '2' but with list of input dirs/files in 'pftree';
            5: As with level '3' but with explicit file logging for
                    - read
                    - analyze
                    - write


Examples
--------

Run down a directory tree and touch all the files in the input tree that are ``jpgs`` to similar locations in the output directory:

.. code:: bash

        pfdo                                                \\
            -I /var/www/html/data -f jpg                    \\
            -O /tmp/jpg --test --json                       \\
            --threads 0 --printElapsedTime


The above will find all files in the tree structure rooted at /var/www/html/data that also contain the string "jpg" anywhere in the filename. For each file found, a corresponding file will be touched in the output directory, in the same tree location as the original input. This touched file will be prefixed with the
string "analyzed-".

Finally the elapsed time and a JSON output are printed.

