Workbench does not modify upstream `fastparquet`. However, upstream
`fastparquet` depends on `pandas` and Workbench does not _want_ `pandas`.
So we fork the project.

To upgrade:

1. `git remote add upstream https://github.com/dask/fastparquet.git`
1. `git fetch upstream`
1. `git merge upstream/master` # setting `version` in setup.py to `(upstream_version+0.0.1)a1`
1. `git tag v(our_version)` # (the `version` that ends in `a1`)
1. `pytest`
1. `python3 setup.py sdist`
1. `twine upload dist/*` (Create a username at https://pypi.org
   and then email `adam@adamhooper.com` to be added to the maintainer list.)
1. `git push origin v(our_version)` # (the `version` that ends in `a1`)
1. `pip3 install --user cibuildwheel && CIBW_SKIP='cp2* cp*i686* cp34* cp35*' cibuildwheel --platform linux --output-dir wheelhouse`

Now update the dependent project to depend on `workbenchdata-fastparquet==(our_version)`
