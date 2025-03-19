
# Release Notes worksheet linalg_frontend

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

## linalg_frontend
### bc breaking
### deprecation
### new features
### improvements
### bug fixes
### performance
### docs
### devs
### Untopiced
- [ROCm] TunableOp use thread-safe getenv functions ([#142274](https://github.com/pytorch/pytorch/pull/142274))
- [ROCm] Fix TunableOp UTs: Rotating Buffer ([#143172](https://github.com/pytorch/pytorch/pull/143172))
- [ROCm] Fix unit test: matmul_offline_mgpu_tunableop ([#143507](https://github.com/pytorch/pytorch/pull/143507))
- [ReduceOps] Add dimension checking for cummin()/cummax(). ([#143920](https://github.com/pytorch/pytorch/pull/143920))
- [Easy] Fix linalg.norm hint message typo ([#144323](https://github.com/pytorch/pytorch/pull/144323))
- [cpu/sorting] Throw an error when trying to sort complex numbers. ([#144113](https://github.com/pytorch/pytorch/pull/144113))
- [Feat]: Add Multithreading support for kleidiai groupwise GEMM kernels ([#144074](https://github.com/pytorch/pytorch/pull/144074))
- Reverting the PR adding Kleidiai-based int4 kernels ([#145392](https://github.com/pytorch/pytorch/pull/145392))
- Revert "Reverting the PR adding Kleidiai-based int4 kernels (#145392)" ([#145505](https://github.com/pytorch/pytorch/pull/145505))
- Fix workarea compute in lapackSyevd ([#146456](https://github.com/pytorch/pytorch/pull/146456))
- Fix logging and test files which misspell "precision" ([#146113](https://github.com/pytorch/pytorch/pull/146113))
- [Quant][CPU] add a wrapper op for _weight_int4pack_mm_for_cpu with tensor args ([#145245](https://github.com/pytorch/pytorch/pull/145245))
### not user facing
- [ROCm] Fix unit test: matmul_offline_tunableop ([#143322](https://github.com/pytorch/pytorch/pull/143322))
- [CUDA] Bump tolerances in `test_svd_lowrank_cuda_float64` ([#143049](https://github.com/pytorch/pytorch/pull/143049))
- [BE][Easy] enable PYFMT for `torch/[a-s]*/` ([#138447](https://github.com/pytorch/pytorch/pull/138447))
- [ARM] Fix bf32 and tf32 precision for tensordot unit test ([#141136](https://github.com/pytorch/pytorch/pull/141136))
- [ROCm][TunableOp] Future proof TunableOp unit test. ([#146548](https://github.com/pytorch/pytorch/pull/146548))
- [ROCm][TunableOp] resolve the rocBLAS version dynamically ([#147363](https://github.com/pytorch/pytorch/pull/147363))
- Also support non-contiguous activation for torch._weight_int8pack_mm on CPU ([#147588](https://github.com/pytorch/pytorch/pull/147588))
- [ROCm][TunableOp] Speed-up matmul_small_brute_force_tunableop unit test ([#147659](https://github.com/pytorch/pytorch/pull/147659))
- [ROCm] [TunableOp] Unit tests for scaled GEMM and GEMM with bias ([#147890](https://github.com/pytorch/pytorch/pull/147890))
- [ROCm][TunableOp] Unit test for offline tuning of GEMM with bias ([#148371](https://github.com/pytorch/pytorch/pull/148371))
- [Test][Linalg][CUDA] Increase niter in test_svd_lowrank_cuda_float64 ([#145930](https://github.com/pytorch/pytorch/pull/145930))
- [ROCm][TunableOp] Add support for rowwise scaling on scaled GEMM. ([#148238](https://github.com/pytorch/pytorch/pull/148238))
- [ROCm][TunableOp] Add bias data type to params signature. ([#146227](https://github.com/pytorch/pytorch/pull/146227))
### security
