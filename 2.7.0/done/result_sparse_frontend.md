
# Release Notes worksheet sparse_frontend

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

## sparse_frontend
### bc breaking
### deprecation
### new features
### improvements
### bug fixes
### performance
- Remove unnecessary tensor `clone`s throughout codebase (#148159)
### docs
### devs
### Untopiced

### not user facing
- [CUDA][64-bit indexing] Fix some existing problematic `int64_t _ = blockIdx.* * blockDim.*` code ([#142010](https://github.com/pytorch/pytorch/pull/142010))
- [BE][Sparse] Get rid of gcc-5 workaround ([#143653](https://github.com/pytorch/pytorch/pull/143653))
- [Intel GPU] Support SparseCsrXPU codegen ([#144722](https://github.com/pytorch/pytorch/pull/144722))
- Add cutlass version guard in prep for upgrade ([#143551](https://github.com/pytorch/pytorch/pull/143551))
- [cutlass-3] Update third-party/cutlass-3 from 3.4 to 3.5.1 ([#143515](https://github.com/pytorch/pytorch/pull/143515))
- Back out "[Submodule] Upgrade to Cutlass 3.6" ([#144738](https://github.com/pytorch/pytorch/pull/144738))
- [Submodule] Upgrade to Cutlass 3.6 part deux ([#144911](https://github.com/pytorch/pytorch/pull/144911))
### security
