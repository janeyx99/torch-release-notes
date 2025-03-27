
# Release Notes worksheet nn_frontend

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

## nn_frontend
### bc breaking
### deprecation
### new features
### improvements
- Add `strict` kwarg to `nn.Module.set_submodule` and fix bug for non dot-delineated strings ([#143455](https://github.com/pytorch/pytorch/pull/143455))
- Improve input dimensions check for `reflection_pad1d`, `reflection_pad2d` and `reflection_pad3d` ([#141670](https://github.com/pytorch/pytorch/pull/141670))
### bug fixes
- Fix broken meta function for flex-attention backwards ([#146563](https://github.com/pytorch/pytorch/pull/146563))
### performance
### docs
- Add clarification for target types in `CrossEntropyLoss` doc ([#145444](https://github.com/pytorch/pytorch/pull/145444))
### devs
### Untopiced
### not user facing
- [CUDA] Check `size` calculation in `ilpReduce` for `softmax` ([#144009](https://github.com/pytorch/pytorch/pull/144009))
- [CUDA][TF32] Add some missing TF32 decorators to `test_nn.py` ([#144592](https://github.com/pytorch/pytorch/pull/144592))
- [Easy] Replace paper description with link to make a concise description. ([#145031](https://github.com/pytorch/pytorch/pull/145031))
- Fix torch.nn.functional.one_hot param num_classes optional description ([#146470](https://github.com/pytorch/pytorch/pull/146470))
- Optimize param `prepend` class reference `torch.nn.Module` ([#148304](https://github.com/pytorch/pytorch/pull/148304))
- Fix rms_norm in fp16/bf16 ([#147203](https://github.com/pytorch/pytorch/pull/147203))
- torch/nn/modules/linear.py: docs: improvements ([#138484](https://github.com/pytorch/pytorch/pull/138484))
- [5/N] Apply Ruff fixes and pyupgrade to Python 3.9 ([#144205](https://github.com/pytorch/pytorch/pull/144205))
- [ROCm] hipblaslt rowwise f8 gemm ([#144432](https://github.com/pytorch/pytorch/pull/144432))
- [inductor triton] Disable incorrect TF32 usage on CUDA capability < 8 ([#145684](https://github.com/pytorch/pytorch/pull/145684))
- [ROCm] miopen benchmark behavior now better aligns with cudnn ([#145294](https://github.com/pytorch/pytorch/pull/145294))
- Generalize mixed precision in DDP ([#146808](https://github.com/pytorch/pytorch/pull/146808))

### security
