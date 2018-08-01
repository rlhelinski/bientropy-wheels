This repository on GitHub is to automate the compilation of binary
wheel packages of BiEntropy for later upload to PyPI.

- https://github.com/sandialabs/bientropy
- https://pypi.org/project/bientropy/

It uses the ``multibuild`` system developed by Matthew Brett and
the MacPython project.

- https://github.com/matthew-brett/multibuild
- https://github.com/MacPython/wiki/wiki/Wheel-building

The builds themselves are performed on virtual machines using
continuous integration testing services. Specifically, TravisCI
for Linux and Apple Mac OS X, and AppVeyor for Microsoft Windows.

- https://travis-ci.org/rlhelinski/bientropy-wheels/builds
- https://ci.appveyor.com/project/rlhelinski/bientropy-wheels/history

These files were based on https://github.com/biopython/biopython-wheels

Building a New Release
----------------------

If you have not already, clone this repository and set up the git submodules:

.. code:: bash

    git clone git@github.com:rlhelinski/bientropy-wheels.git
    cd bientropy-wheels/
    git submodule init
    git submodule update

The procedure for compiling the wheels for a new release are as follows:

1.  Update the version string in the ``setup.py`` file in ``bientropy`` and tag
    that revision ``<new tag>``. Be sure to push this version and the tag, or
    the CI runners will not be able to see it.

.. code:: bash

    git tag <new tag>
    git push --tags

2.  Switch the ``bientropy`` revision referenced by the git submodule in
    ``bientropy-wheels`` to the revision you want to build:

.. code:: bash

    cd bientropy-wheels/bientropy
    git fetch
    git checkout <new tag>
    cd ..
    git add bientropy

3.  Commit the new revision reference in the ``bientropy-wheels`` repo and push
    to initiate the CI builds.

.. code:: bash

    git commit ...
    git push

4.  Travis CI does not have an artifacts feature like AppVeyor does. Therefore,
    the Travis CI jobs will attempt to upload the wheel files to TestPyPI if
    the revision is tagged.  To do this, tag the revision before the ``push``
    in the previous step and be sure to push the tag:

.. code:: bash

    git tag <new tag>
    git push --tags

The git tag does not necessarily need to coincide with the tag that will be
inside and part of the name of the wheel files. That version should be the same
as the one from ``setup.py``.

If you need to change the remote path to one of the submodules, e.g. to
reference a fork, then edit the ``.gitmodules`` file, commit that change and
run the following commands:

.. code:: bash

    git submodule sync
    git submodule update
