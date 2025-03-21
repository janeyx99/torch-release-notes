
# Release Notes worksheet cuda

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

## cuda
### bc breaking
### deprecation
### new features
- [NVIDIA] RTX50 Blackwell Support codegen ([#145270](https://github.com/pytorch/pytorch/pull/145270))
- Make torch.cuda.gds APIs public ([#147120](https://github.com/pytorch/pytorch/pull/147120))
### improvements
- Refine CUDA Stream priority ([#143849](https://github.com/pytorch/pytorch/pull/143849))
- Expose sharedMemPerMultiprocessor device property to python ([#143119](https://github.com/pytorch/pytorch/pull/143119))
- Expose remaining sharedMem cudaDeviceProps to python ([#143226](https://github.com/pytorch/pytorch/pull/143226))
### bug fixes
- Let PYTORCH_NO_CUDA_MEMORY_CACHING has effect only when value is 1 ([#145905](https://github.com/pytorch/pytorch/pull/145905))
### performance
- Add option to limit number of SMs used by matmul kernels ([#144974](https://github.com/pytorch/pytorch/pull/144974))
- Add option to limit number of SMs used by matmul kernels ([#147966](https://github.com/pytorch/pytorch/pull/147966))
### docs
- [BE] correct docs for clock_rate to MHz, fixes #147098 ([#147393](https://github.com/pytorch/pytorch/pull/147393))
### devs
### Untopiced
- [ROCm] Improve performance of reduce sum for 3D shapes ([#143137](https://github.com/pytorch/pytorch/pull/143137))
- [torch][cuda] fix race condition in cuda initialization ([#143238](https://github.com/pytorch/pytorch/pull/143238))
- fix a few int64_t index computations, fix complex128 scan that had to… ([#143401](https://github.com/pytorch/pytorch/pull/143401))
- Add range check embedding_bag on input index >= 0 of cuda device ([#140791](https://github.com/pytorch/pytorch/pull/140791))
- [ROCm][NFC] Fix condition for small tensor tuning ([#144087](https://github.com/pytorch/pytorch/pull/144087))
- use cooperative schedule in scaled_mm for fast_accum=false ([#144809](https://github.com/pytorch/pytorch/pull/144809))
- fix acquire pattern in topk ([#144945](https://github.com/pytorch/pytorch/pull/144945))
- Improve softmax's perf in cuda ([#144679](https://github.com/pytorch/pytorch/pull/144679))
- Removes threadfence from topk kernel to improve AMD performance ([#145536](https://github.com/pytorch/pytorch/pull/145536))
- [64-bit] Int64 casting for UpSampleNearest3D ([#144865](https://github.com/pytorch/pytorch/pull/144865))
- initialize device when pinning memory on this device, short circuit i… ([#145752](https://github.com/pytorch/pytorch/pull/145752))
- [cuda] fix printing of num_gpus ([#146838](https://github.com/pytorch/pytorch/pull/146838))
- [torch] Make amdsmi cdll hook private ([#147207](https://github.com/pytorch/pytorch/pull/147207))
- Fix linter warnings ([#147386](https://github.com/pytorch/pytorch/pull/147386))
- Add cutlass kernel for rowwise scaled mm on sm100 ([#148421](https://github.com/pytorch/pytorch/pull/148421))
- Add get_stream_from_external API for CUDA backend ([#143799](https://github.com/pytorch/pytorch/pull/143799))
- Support with statement on torch.Stream ([#140138](https://github.com/pytorch/pytorch/pull/140138))

### not user facing
- [ROCM] Enable *_load_dwordx4 ISA for BFloat16 and Half. ([#141397](https://github.com/pytorch/pytorch/pull/141397))
- [ATen][Native][CUDA][SCALED_MM] limit f8f8bf16 rowwise scaled matmul to sm_90 ([#145728](https://github.com/pytorch/pytorch/pull/145728))
- [ROCm] Tune 3d tensor sums when not using fastest dimension ([#146170](https://github.com/pytorch/pytorch/pull/146170))
- [CUDA][B200] Update the number of threads in `avg_pool2d` backward for SM 10.0 ([#145669](https://github.com/pytorch/pytorch/pull/145669))
- [ROCm][Windows] Fix isnan integer overload errors on MS STL ([#146605](https://github.com/pytorch/pytorch/pull/146605))
- [cuda] Simplify the sinc function a bit. ([#146774](https://github.com/pytorch/pytorch/pull/146774))
- [ROCm] Optimize the stride one indexing backwards kernel ([#146420](https://github.com/pytorch/pytorch/pull/146420))
- [ROCm] Use IPT=8 for block radix sort ([#147657](https://github.com/pytorch/pytorch/pull/147657))
### security
