
# Release Notes worksheet dataloader_frontend

The main goal of this process is to rephrase all the commit messages below to make them **clear and easy to read** by the end user. You should follow the following instructions to do so:

* **Please cleanup, and format commit titles to be readable by the general PyTorch user.** Make sure you're [following the guidance here](https://docs.google.com/document/d/14OmgGBr1w6gl1VO47GGGdwrIaUNr92DFhQbY_NEk8mQ/edit)! Your resulting notes must be consistent and easy to read.
* Please sort commits into the following categories (you should not rename the categories!), I tried to pre-sort these to ease your work, feel free to move commits around if the current categorization is not good.
* Anything that is not public facing needs to be removed.
* If anything is miscategorized/belongs to another domain, move it to `miscategorized.md`.
* Please scan through `miscategorized.md` and handle any commits that belong within your domain according to these instructions.
* Please use markdown format.
* Please use #PR_NUM to link to the PR, instead of `[#PR_NUM](https://github.com/pytorch/pytorch/pull/#PR_NUM)` to reduce the length of the release notes.
* We place a lot of emphasis on the “BC-breaking” and “deprecation” sections. Those should be where the most effort goes in. The “improvements” and “bug fixes” for Python API should be nice as well.
* Once you are finished, move this very file from `todo/` to `done/` and submit a pull request.

The categories below are as follows:

* BC breaking: All commits that are BC-breaking. These are the most important commits. If any pre-sorted commit is actually BC-breaking, do move it to this section. Each commit should contain a paragraph explaining the rational behind the change as well as an example for how to update user code [BC-Guidelines](https://docs.google.com/document/d/14OmgGBr1w6gl1VO47GGGdwrIaUNr92DFhQbY_NEk8mQ/edit#heading=h.a9htwgvvec1m).
* Deprecations: All commits introducing deprecation. Each commit should include a small example explaining what should be done to update user code.
* new_features: All commits introducing a new feature (new functions, new submodule, new supported platform etc)
* improvements: All commits providing improvements to existing feature should be here (new backend for a function, new argument, better numerical stability)
* bug fixes: All commits that fix bugs and behaviors that do not match the documentation
* performance: All commits that are added mainly for performance (we separate this from improvements above to make it easier for users to look for it)
* documentation: All commits that add/update documentation
* Developers: All commits that are not end-user facing but still impact people that compile from source, develop into pytorch, extend pytorch, etc
* not user facing: All commits that are not public end-user facing and hence should be dropped from the release notes

## dataloader_frontend
### bc breaking
### deprecation
### new features
### improvements
### bug fixes
### performance
### docs
### devs
### Untopiced
- Dataloader distribute tasks to workers when in_order is False ([#142324](https://github.com/pytorch/pytorch/pull/142324))
- Update pin memory related APIs to not pass 'device' argument ([#131858](https://github.com/pytorch/pytorch/pull/131858))
### not user facing
- remove allow-untyped-defs for utils/data/datapipes/dataframe/structures.py ([#143273](https://github.com/pytorch/pytorch/pull/143273))
- Use default_collate from public API ([#143616](https://github.com/pytorch/pytorch/pull/143616))
- remove allow-untyped-defs from utils/data/datapipes/iter/streamreader.py ([#144088](https://github.com/pytorch/pytorch/pull/144088))
- Migrate from Tuple -> tuple in torch/utils/data ([#144255](https://github.com/pytorch/pytorch/pull/144255))
- Backout PEP585 use of Iterable ([#145438](https://github.com/pytorch/pytorch/pull/145438))
- Enable some tests on MacOS ([#146268](https://github.com/pytorch/pytorch/pull/146268))
- Remove unactivated test ([#146233](https://github.com/pytorch/pytorch/pull/146233))
- Fixed a typo in dataset.py ([#146600](https://github.com/pytorch/pytorch/pull/146600))
- Fix PEP585 update ([#147536](https://github.com/pytorch/pytorch/pull/147536))
### security
