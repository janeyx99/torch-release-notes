
# Release Notes worksheet caffe2

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

## caffe2
### bc breaking
### deprecation
### new features
### improvements
### bug fixes
### performance
### docs
### devs
### Untopiced

### not user facing
- [caffe2] Move vectorized templates into a separate file for box_cox operator ([#143556](https://github.com/pytorch/pytorch/pull/143556))
- [Codemod][AddExplicitStrictExportArg] caffe2/torch/onnx/_internal/exporter ([#143542](https://github.com/pytorch/pytorch/pull/143542))
- [Codemod][AddExplicitStrictExportArg] caffe2/benchmarks/dynamo ([#143686](https://github.com/pytorch/pytorch/pull/143686))
- [Codemod][AddExplicitStrictExportArg] caffe2/test/inductor ([#143929](https://github.com/pytorch/pytorch/pull/143929))
- Fix ruff warnings in caffe2 and functorch ([#144182](https://github.com/pytorch/pytorch/pull/144182))
- [caffe2] Add AVX512 support for box_cox operator ([#143627](https://github.com/pytorch/pytorch/pull/143627))
- [caffe2] Use the manifold cache backend as the default ([#144773](https://github.com/pytorch/pytorch/pull/144773))
- Add "//caffe2:libtorch"  to minifier TARGET file ([#146203](https://github.com/pytorch/pytorch/pull/146203))
- [Codemod][AddExplicitStrictExportArg] caffe2/torch ([#146439](https://github.com/pytorch/pytorch/pull/146439))
- [codemod] Fix unused-value issue in caffe2/aten/src/ATen/native/miopen/Conv_miopen.cpp +1 ([#147496](https://github.com/pytorch/pytorch/pull/147496))
- [caffe2] Ignore compiler option when building using clang ([#147556](https://github.com/pytorch/pytorch/pull/147556))
- [codemod] Fix missing field initializer in caffe2/torch/lib/libshm/manager.cpp +1 ([#148393](https://github.com/pytorch/pytorch/pull/148393))
- [Codemod][AddExplicitStrictExportArg] caffe2/test/inductor ([#148781](https://github.com/pytorch/pytorch/pull/148781))
- [Codemod][AddExplicitStrictExportArg] caffe2/test/inductor ([#148928](https://github.com/pytorch/pytorch/pull/148928))
- Fix unused-variable issues in caffe2 ([#143639](https://github.com/pytorch/pytorch/pull/143639))
- Fix unused-variable issues in caffe2 ([#143639](https://github.com/pytorch/pytorch/pull/143639))
- [caffe2] disable warning for unused arguments ([#147411](https://github.com/pytorch/pytorch/pull/147411))
- [caffe2/torch] Fixup upstream LLVM (major version 21) API changes ([#148833](https://github.com/pytorch/pytorch/pull/148833))
### security
