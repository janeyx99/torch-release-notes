
# Release Notes worksheet cpp_frontend

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

## cpp_frontend
### bc breaking
### deprecation
### new features
- Support libtorch-agnostic extensions with stable torch ABI ([#148892](https://github.com/pytorch/pytorch/pull/148892), [#148832](https://github.com/pytorch/pytorch/pull/148832), [#148124](https://github.com/pytorch/pytorch/pull/148124))
### improvements
- Introduce a new API `isAcceleratorExcluded` ([#144959](https://github.com/pytorch/pytorch/pull/144959))

### bug fixes
- Fix Apple Clang ICE when building with -march=armv8.6a ([#142879](https://github.com/pytorch/pytorch/pull/142879))
- Fix inductor regression on aarch64 neoverse-v1 with gcc10.2 by disabling tree vectorization ([#148489](https://github.com/pytorch/pytorch/pull/148489))
### performance
### docs
### devs
### Untopiced
### not user facing
- [codemod] Fix a few unused-variable issues in pytorch ([#143517](https://github.com/pytorch/pytorch/pull/143517))
- [rpc] Fix unit test after c10::nullopt removal ([#143690](https://github.com/pytorch/pytorch/pull/143690))
- [codemod] Remove unused-variable in caffe2/aten/src/ATen/native/quantized/cpu/fbgemm_utils.cpp +1 ([#144783](https://github.com/pytorch/pytorch/pull/144783))
- [codemod] Fix unused-value issue in caffe2/aten/src/ATen/cuda/detail/CUDAHooks.cpp +4 ([#147555](https://github.com/pytorch/pytorch/pull/147555))
- [codemod] Remove unused-variable in caffe2/torch/csrc/distributed/c10d/cuda/AsyncMM.cu ([#148501](https://github.com/pytorch/pytorch/pull/148501))
- c10::optional -> std::optional ([#142514](https://github.com/pytorch/pytorch/pull/142514))
- c10::string_view -> std::string_view in more places ([#142517](https://github.com/pytorch/pytorch/pull/142517))
- Disable c10::optional macros ([#138912](https://github.com/pytorch/pytorch/pull/138912))
- Fix issue with setAttribute and int8_t vs int32_t variables ([#143693](https://github.com/pytorch/pytorch/pull/143693))
- Fix issue with setAttribute and int8_t vs int32_t variables ([#143693](https://github.com/pytorch/pytorch/pull/143693))
- c10::string_view -> std::string_view in Device.cpp ([#144178](https://github.com/pytorch/pytorch/pull/144178))
- c10::optional -> std::optional in a few places ([#144340](https://github.com/pytorch/pytorch/pull/144340))
- [4/N] Apply bugprone-unchecked-optional-access  ([#142832](https://github.com/pytorch/pytorch/pull/142832))
- Fix old-compiler-unfriendly zero init of bfloat16_t array ([#143504](https://github.com/pytorch/pytorch/pull/143504))
- Enable more readability-redundant checks ([#143963](https://github.com/pytorch/pytorch/pull/143963))
- Enable readability-redundant-declaration ([#143982](https://github.com/pytorch/pytorch/pull/143982))
- [18/N] Fix extra warnings brought by clang-tidy-17 ([#144014](https://github.com/pytorch/pytorch/pull/144014))
- [19/N] Fix extra warnings brought by clang-tidy-17 ([#144448](https://github.com/pytorch/pytorch/pull/144448))
- Enable bugprone-unchecked-optional-access ([#144226](https://github.com/pytorch/pytorch/pull/144226))
- [2/N] Remove NOLINT suppressions ([#146402](https://github.com/pytorch/pytorch/pull/146402))
- Fix erroneous at_vreinterpretq_u16_bf16 call ([#144883](https://github.com/pytorch/pytorch/pull/144883))
- Remove C10_EMBEDDED ([#144808](https://github.com/pytorch/pytorch/pull/144808))
### security
