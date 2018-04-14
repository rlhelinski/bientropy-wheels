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

The procedure for compiling the wheels for a new release are as follows:

1.  Update the version string in the ``setup.py`` file in ``bientropy`` and tag
    that revision ``<new tag>``.
2.  Switch the ``bientropy`` revision referenced by the git submodule in
    ``bientropy-wheels`` to that revision. e.g.,:

.. code:: bash

    cd bientropy-wheels; cd bientropy
    git fetch; git checkout <new tag>
    cd ..;
    git add bientropy

3.  ``git commit`` ...; ``git tag`` <new tag> ; ``git push``
    The push will initiate the builds. The Travis CI jobs will try to upload to
    PyPI because the revision is tagged.

The git tag does not necessarily need to coincide with the tag that will be
inside and part of the name of the wheel files. That version will be the one
from 'setup.py'.
