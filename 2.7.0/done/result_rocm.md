
# Release Notes worksheet rocm

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

## rocm
### bc breaking
### deprecation
### new features
- CK Memory-Efficient Attention (attention bias support) (#147778)
- CK Flash Attention Backend (#143695)
- Enhanced Windows support for PyTorch on ROCm (#148563, #144098)
- Support for gfx1102 arch (Navi33) in wheel builds (#147761)
- hipblaslt rowwise f8 gemm (#144432)
### improvements
- Fix TunableOp UTs: Rotating Buffer (#143172)
- Enable *_load_dwordx4 ISA for BFloat16 and Half. ([#141397](https://github.com/pytorch/pytorch/pull/141397))
- Fix condition for small tensor tuning ([#144087](https://github.com/pytorch/pytorch/pull/144087))

### bug fixes
- TunableOp use thread-safe getenv functions (#142274)
- fix torch.layer_norm invalid configuration problem when input is large tensor (#144007)
- [Inductor][CK] hackfix for segfault in `addmm` op (#144519)
- Fix `torch.layer_norm` invalid configuration when input is large tensor ([#144007](https://github.com/pytorch/pytorch/pull/144007))
- Fix `isnan` integer overload errors on MicroSoft STL ([#146605](https://github.com/pytorch/pytorch/pull/146605))
### performance
- Improve backwards indexing when stride is not one (#147630)
- Improvements for vectorized elementwise kernels (#143269)
- Skip L1 cache for single-use buffers in tl.load (#143115)
- Improve performance of reduce sum for 3D shapes (#143137)
- Enable `_load_dwordx4` ISA for BFloat16 and Half (#141397)
- Improve reduce sum calculation for low CU count (#141378)
- Tune 3d tensor sums when not using fastest dimension ([#146170](https://github.com/pytorch/pytorch/pull/146170))
- Optimize the stride one indexing backwards kernel ([#146420](https://github.com/pytorch/pytorch/pull/146420))
- Use IPT=8 for block radix sort ([#147657](https://github.com/pytorch/pytorch/pull/147657))
- Improve performance of reduce sum for 3D shapes ([#143137](https://github.com/pytorch/pytorch/pull/143137))

### docs
### devs
### Untopiced

### not user facing
- Update ck ([#144799](https://github.com/pytorch/pytorch/pull/144799))
- CK SDPA - Move arch check to CK patch ([#144777](https://github.com/pytorch/pytorch/pull/144777))
- Simplify the `sinc` function a bit. ([#146774](https://github.com/pytorch/pytorch/pull/146774))
- Make amdsmi cdll hook private ([#147207](https://github.com/pytorch/pytorch/pull/147207))
### security
