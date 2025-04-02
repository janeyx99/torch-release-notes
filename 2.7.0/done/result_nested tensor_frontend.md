
# Release Notes worksheet nested tensor_frontend

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

## nested tensor_frontend
### bc breaking
### deprecation
### new features
### improvements
- Support NJT `chunk()` backward on batch dim ([#144584](https://github.com/pytorch/pytorch/pull/144584))
- Support remaining `*_like` factory functions for NJT ([#144889](https://github.com/pytorch/pytorch/pull/144889))
- Improve `matmul` with NJTs via backward support and composition with dense tensors ([#144587](https://github.com/pytorch/pytorch/pull/144587), [#146405](https://github.com/pytorch/pytorch/pull/146405))
### bug fixes
- Fix NJT `min` / `max` backward() for non-ragged reductions ([#144583](https://github.com/pytorch/pytorch/pull/144583))
- Fix NJT `frexp()` to handle both outputs ([#144585](https://github.com/pytorch/pytorch/pull/144585))
- Fix NJT `fill.Scalar` for contiguous inputs ([#144586](https://github.com/pytorch/pytorch/pull/144586))
- Fix inference mode for composite implicit ops without nested-specific kernel ([#146633](https://github.com/pytorch/pytorch/pull/146633))
- Fix flop counter for SDPA and test ([#147032](https://github.com/pytorch/pytorch/pull/147032))
### performance
### docs
- Update OSS nested tensor docs to focus on NJT ([#145402](https://github.com/pytorch/pytorch/pull/145402))
### devs
### Untopiced
### not user facing
### security
