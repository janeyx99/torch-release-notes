
# Release Notes worksheet autograd_frontend

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

## autograd_frontend
### bc breaking
### deprecation
### new features
### improvements
- Allow GradientEdge as torch.autograd.backward outputs ([#144744](https://github.com/pytorch/pytorch/pull/144744))
### bug fixes
- Fix allow_mutation_on_saved_tensors for inplace foreach ([#145520](https://github.com/pytorch/pytorch/pull/145520))
### performance
### docs
### devs
### Untopiced
- [BE] Actually suppress vmap warning from gradcheck ([#144287](https://github.com/pytorch/pytorch/pull/144287))
- Revert D70262395 ([#148164](https://github.com/pytorch/pytorch/pull/148164))
- Implement gradient for the `residuals` of `torch.linalg.lstsq` ([#148526](https://github.com/pytorch/pytorch/pull/148526))
- [reland][ca] side-effect free inital trace: compiled_args ([#148376](https://github.com/pytorch/pytorch/pull/148376))
### not user facing
- [3/N] Apply py39 ruff fixes ([#142115](https://github.com/pytorch/pytorch/pull/142115))
- [gen_autograd_functions] rename some variables ([#143166](https://github.com/pytorch/pytorch/pull/143166))
- [Submodule] Turning flash-attention integration into 3rd party submod (#144120) ([#146372](https://github.com/pytorch/pytorch/pull/146372))
### security
