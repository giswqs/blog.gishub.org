## How to publish a Python package on conda-forge

**Here is a quick start guide for publishing a Python package on conda-forge. More information can be found in this [video tutorial](https://youtu.be/NNWhz7pqwCw). **

1. Install conda-build using: `conda install conda-build`
2. Create the conda recipe using: `conda skeleton pypi package-name`
3. Make changes to the recipe (**meta.yaml**) by following this [example](https://github.com/giswqs/geodemo/blob/master/recipe/meta.yaml). A few key items to change: add `noarch: python` under the `build` section; remove all packages except `pip` and `python` from the `host` section; specify a python version (e.g., >=3.6) for both the `host` and `run` sections; add `LICENSE`, `doc_url`, and `dev_url` to the `about` section; add your GitHub username to the `recipe-maintainers` section.
4. Fork <https://github.com/conda-forge/staged-recipes>
5. Add your package recipe to `staged-recipes/recipes/package-name/meta.yaml`
6. Commit changes and push to GitHub
7. Submit a pull request (e.g., <https://github.com/conda-forge/staged-recipes/pull/14285>)
8. Wait for the recipe to pass all checks
9. `@conda-forge/help-python` to let them know that your recipe is ready for review.
10. Once your recipe is accepted, your package feedstock should become available within a few hours (e.g., [geodemo-feedstock](https://github.com/conda-forge/geodemo-feedstock)).
11. The link to your package on conda-forage should be <https://anaconda.org/conda-forge/package-name>, such as <https://anaconda.org/conda-forge/geodemo>

%[https://youtu.be/NNWhz7pqwCw]