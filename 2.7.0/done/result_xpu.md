
# Release Notes worksheet xpu

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

## xpu
### bc breaking
### deprecation

### New Features
- Add AOT Inductor support for Intel GPU ([#140269](https://github.com/pytorch/pytorch/pull/140269), [#140664](https://github.com/pytorch/pytorch/pull/140664))
- Optimize SDPA Inference Performance for XPU ([#147614](https://github.com/pytorch/pytorch/pull/147614), [#147612](https://github.com/pytorch/pytorch/pull/147612))
- Support `torch.compile` on Windows Platform for XPU ([#147637](https://github.com/pytorch/pytorch/pull/147637), [#144316](https://github.com/pytorch/pytorch/pull/144316))
- [XPU] Support SYCL with `torch.utils.cpp_extension` APIs ([#132945](https://github.com/pytorch/pytorch/pull/132945))

### Improvements
- [Intel GPU] Enable FP64 GEMM ([#140677](https://github.com/pytorch/pytorch/pull/140677))
- [Intel GPU] allow_tf32 for oneDNN backend - XPU part ([#137570](https://github.com/pytorch/pytorch/pull/137570))
- [Intel GPU] Support SparseCsrXPU codegen ([#144722](https://github.com/pytorch/pytorch/pull/144722))
- [XPU] Add the API to get Stream from external libraries for XPU backend ([#141123](https://github.com/pytorch/pytorch/pull/141123))
- [XPU] Add low priority XPU Stream ([#141119](https://github.com/pytorch/pytorch/pull/141119))
- [XPU] Enable XPU for Inductor MM Triton Kernel Benchmark ([#148237](https://github.com/pytorch/pytorch/pull/148237))
- [Intel GPU] Decompose Intel GPU oneDNN from other backends ([#147926](https://github.com/pytorch/pytorch/pull/147926))
- [Intel GPU] Avoid including CPU oneDNN header files for Intel GPU ([#147969](https://github.com/pytorch/pytorch/pull/147969))
- [Intel GPU] Align XPU convolution_backward output layout between fake tensor and real output tensor ([#146880](https://github.com/pytorch/pytorch/pull/146880))
- [XPU] Improve error handling and reporting in CMake files ([#149353](https://github.com/pytorch/pytorch/pull/149353))
- [XPU] Refine XPU oneDNN context manager API ([#147349](https://github.com/pytorch/pytorch/pull/147349))
- [XPU] Refine XPU external Stream ([#142347](https://github.com/pytorch/pytorch/pull/142347))
- [XPU] Refine torch.xpu.get_device_properties API error message ([#144379](https://github.com/pytorch/pytorch/pull/144379))
- [Intel GPU] Add XPU device to nested_layer_norm ([#148593](https://github.com/pytorch/pytorch/pull/148593))
- [XPU] Generalize `is_big_gpu()` check in Inductor ([#143491](https://github.com/pytorch/pytorch/pull/143491))

### Bug Fixes
- [Intel GPU] Fix SDPA dummy LSE output to match meta function ([#148652](https://github.com/pytorch/pytorch/pull/148652))
- [Intel GPU] Fix memory leak in deconv backward ([#144385](https://github.com/pytorch/pytorch/pull/144385))
- [Intel GPU] Fix bug: use zero-point to decide conv src zp mask ([#149473](https://github.com/pytorch/pytorch/pull/149473))
- [XPU] Fix issue #143489 in torch.utils._content_store hash storage ([#147785](https://github.com/pytorch/pytorch/pull/147785))
- [XPU] Fix Inductor UT failures introduced from community ([#146762](https://github.com/pytorch/pytorch/pull/146762))
- [XPU] Fix broken XPU CI introduced by community changes ([#145058](https://github.com/pytorch/pytorch/pull/145058))
- [XPU] Fix Inductor UT on Windows ([#146481](https://github.com/pytorch/pytorch/pull/146481))
- [XPU] Fix missing `model_container_runner_xpu.cpp` in `libtorch_xpu.so` ([#149175](https://github.com/pytorch/pytorch/pull/149175))

### Performance
- [Intel GPU] Optimize SDPA Inference Performance for XPU ([#147614](https://github.com/pytorch/pytorch/pull/147614), [#147612](https://github.com/pytorch/pytorch/pull/147612))
- [Intel GPU] Improve zero-point memory creation ([#148640](https://github.com/pytorch/pytorch/pull/148640))
- [Intel GPU] Optimize qlinear.pointwise with mixed dtype support ([#136753](https://github.com/pytorch/pytorch/pull/136753))
- [Intel GPU] Optimize qconv.pointwise with mixed dtype XPU support ([#135465](https://github.com/pytorch/pytorch/pull/135465))
- [Intel GPU] Optimize qlinear_pointwise.binary[_tensor] XPU support ([#135337](https://github.com/pytorch/pytorch/pull/135337))
- [Intel GPU] Optimize qconv_pointwise.binary XPU support ([#135189](https://github.com/pytorch/pytorch/pull/135189))
- [Intel GPU] Use oneDNN v3.7.1 for better performance ([#148403](https://github.com/pytorch/pytorch/pull/148403))
- [Intel GPU] Avoid unnecessary copy when the dst of Matmul is non-contiguous ([#144759](https://github.com/pytorch/pytorch/pull/144759))
- [Intel GPU] Avoid copy when the input of Matmul is broadcasted ([#143784](https://github.com/pytorch/pytorch/pull/143784))
- [Intel GPU] Convert Conv1D to 2D in inductor ([#144140](https://github.com/pytorch/pytorch/pull/144140))

### Documentation
- [XPU] Update AOTInductor documentation for XPU support ([#149299](https://github.com/pytorch/pytorch/pull/149299))
- [XPU] Add note to get started with XPU ([#148168](https://github.com/pytorch/pytorch/pull/148168))
- [XPU] Update "Getting Started on Intel GPU" hardware requirements for FP64 ([#147802](https://github.com/pytorch/pytorch/pull/147802))
- [XPU] Improve SYCL extension documentation ([#147988](https://github.com/pytorch/pytorch/pull/147988))
- [XPU] Address source code building command for Intel GPU support ([#143476](https://github.com/pytorch/pytorch/pull/143476))

### Developer
- [CD] Upgrade XPU runtime PyPI packages and enable Windows Kineto again ([#148319](https://github.com/pytorch/pytorch/pull/148319))
- [CD] Upgrade Windows XPU support package for binary compression ([#148313](https://github.com/pytorch/pytorch/pull/148313))
- [CD] Remove redundant Triton dependency from XPU wheels ([#143839](https://github.com/pytorch/pytorch/pull/143839))
- [CD] Add Python 3.13 build for XPU ([#146614](https://github.com/pytorch/pytorch/pull/146614))
- [CI] Add XPU Linux build into pull workflow ([#145084](https://github.com/pytorch/pytorch/pull/145084))
- [CI] Move XPU Triton build to manylinux 2.28 ([#148195](https://github.com/pytorch/pytorch/pull/148195))
- [CI] Remove manylinux builds for Triton, except for XPU ([#148129](https://github.com/pytorch/pytorch/pull/148129))
- [CI] Unify XPU Windows CI/CD installation scripts ([#143185](https://github.com/pytorch/pytorch/pull/143185))
- [CI] Disable sccache for XPU test ([#143851](https://github.com/pytorch/pytorch/pull/143851))
- [CI] Fix XPU Linux CD wheel test failures ([#143268](https://github.com/pytorch/pytorch/pull/143268))
- [CI] Fix Windows Inductor UT unloading Triton pyd files ([#148323](https://github.com/pytorch/pytorch/pull/148323))
- [CI] Set RUNPATH on CUDA and XPU tests ([#144305](https://github.com/pytorch/pytorch/pull/144305))
- [CI] Fix Inductor UT failures on Windows ([#147347](https://github.com/pytorch/pytorch/pull/147347))
- [XPU] Allow XPU device for validating sparse compressed tensor factory functions ([#147306](https://github.com/pytorch/pytorch/pull/147306))

### Untopiced
### not user facing
### security
