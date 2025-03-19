
# Release Notes worksheet mps

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

## mps
### bc breaking
### deprecation
### new features
### improvements
- [MPS] Support torch.accelerator.synchronize() on mps ([#143171](https://github.com/pytorch/pytorch/pull/143171))
- [Inductor] Implement primitive Metal compiler ([#143893](https://github.com/pytorch/pytorch/pull/143893))
- [MPSInductor] Fix index generation for transpose ([#143973](https://github.com/pytorch/pytorch/pull/143973))
- [MPSInductor] Fix multi rangevar kernel invocation ([#144050](https://github.com/pytorch/pytorch/pull/144050))
- [MPS] Expose `MPSProfiler::start/stopCapture`  to Python ([#144561](https://github.com/pytorch/pytorch/pull/144561))
- [MPSInductor] Better error when kernel fails to compile ([#144649](https://github.com/pytorch/pytorch/pull/144649))
- Add fused rms_norm implementation for MPS backend ([#145301](https://github.com/pytorch/pytorch/pull/145301))
- [MPS][BE] Implement bilineard2d as shader ([#145581](https://github.com/pytorch/pytorch/pull/145581))
- [MPS] cholesky implementation ([#145701](https://github.com/pytorch/pytorch/pull/145701))
- [MPS] Extend `torch.mm`/`torch.bmm` to integral types ([#145809](https://github.com/pytorch/pytorch/pull/145809))
- [MPS] lu factor ex implementation ([#144651](https://github.com/pytorch/pytorch/pull/144651))
- [MPS] Add error checking when dispatching kernel ([#146458](https://github.com/pytorch/pytorch/pull/146458))
- [MPS] Implement support for zeta (both eager and inductor). ([#146465](https://github.com/pytorch/pytorch/pull/146465))
- [MPS] linalg solve implementation ([#146531](https://github.com/pytorch/pytorch/pull/146531))
- [MPS] Extend `torch.special.sinc` to complex ([#146648](https://github.com/pytorch/pytorch/pull/146648))
- [MPS] lu unpack ([#146681](https://github.com/pytorch/pytorch/pull/146681))
- [MPS] Add bilineard2d_aa implementation ([#145526](https://github.com/pytorch/pytorch/pull/145526))
- [BE] Toward Metal Iterator (step 2) ([#147023](https://github.com/pytorch/pytorch/pull/147023))
- [BE] Turn nextafter into functor ([#147018](https://github.com/pytorch/pytorch/pull/147018))
- [MPS] cholesky ex version ([#146799](https://github.com/pytorch/pytorch/pull/146799))
- [MPS] faster integer matmul for mps ([#147526](https://github.com/pytorch/pytorch/pull/147526))
- [MPS] add slogdet and logdet implementations to mps ([#148287](https://github.com/pytorch/pytorch/pull/148287))
- [MPS][BE] Align bitshift behavior with CPU ([#148719](https://github.com/pytorch/pytorch/pull/148719))
- [MPS] Make `torch.mps.compile_shader` public ([#148972](https://github.com/pytorch/pytorch/pull/148972))
- [MPSInductor] Fix large prod and sum reductions ([#148975](https://github.com/pytorch/pytorch/pull/148975))
### bug fixes
- Workaround for gather_out in MPS backend ([#135543](https://github.com/pytorch/pytorch/pull/135543))
- [MPS] Fix fmin/fmax for scalar argument ([#143934](https://github.com/pytorch/pytorch/pull/143934))
- [MPS] Fix crash when mm is invoked with mixed dtypes ([#143948](https://github.com/pytorch/pytorch/pull/143948))
- [MPS] Fix `nllnd_loss_backward` crash with different dtypes ([#144170](https://github.com/pytorch/pytorch/pull/144170))
- [MPSInductor] Fix `masked`/`where` for inf values ([#144500](https://github.com/pytorch/pytorch/pull/144500))
- [MPS] Make sure that MPSStream is usable from C++ ([#144559](https://github.com/pytorch/pytorch/pull/144559))
- [MPS] Make MPSProfiler usable from C++ ([#144560](https://github.com/pytorch/pytorch/pull/144560))
- [MPS] Fix regression in con-contig bitwise ops ([#146085](https://github.com/pytorch/pytorch/pull/146085))
- [MPS] fix lu factor for large tensors with bs>1 ([#146753](https://github.com/pytorch/pytorch/pull/146753))
- bug fix: ensure 4d input in _scaled_dot_product_attention_math_mps ([#146623](https://github.com/pytorch/pytorch/pull/146623))
- [MPS] Fix cholesky_ex for empty inputs ([#147159](https://github.com/pytorch/pytorch/pull/147159))
- [MPS] fix attention for >4d tensors ([#147545](https://github.com/pytorch/pytorch/pull/147545))
- [MPS] Implemented `masked_fill_scalar` as shader ([#147369](https://github.com/pytorch/pytorch/pull/147369))
- [MPS] fix empty place holder error for smooth l1 loss ([#148133](https://github.com/pytorch/pytorch/pull/148133))
- [MPS] Fix sqrt and other for `torch.chalf` ([#148285](https://github.com/pytorch/pytorch/pull/148285))
- [MPS] Fix unary_kernel_strided logic ([#148512](https://github.com/pytorch/pytorch/pull/148512))
- [MPS] fix crash for mse loss with 0 numel inputs ([#148608](https://github.com/pytorch/pytorch/pull/148608))
- [MPS] Fix scalar to tensors bitshifts ([#148686](https://github.com/pytorch/pytorch/pull/148686))
- [MPSInductor] Fix `min`/`max` reductions over large dims ([#149004](https://github.com/pytorch/pytorch/pull/149004))
- [MPSInductor][EZ] Fix argmin/max signatures ([#149020](https://github.com/pytorch/pytorch/pull/149020))
- [MPS] Enable angle and atan2 for `torch.long` ([#149017](https://github.com/pytorch/pytorch/pull/149017))
### performance
- [MPS] faster integer batched matmul ([#147877](https://github.com/pytorch/pytorch/pull/147877))
- [MPS] Implement linear1d as shader ([#148154](https://github.com/pytorch/pytorch/pull/148154))
- [MPS] metal unary kernel for sqrt ([#148272](https://github.com/pytorch/pytorch/pull/148272))
- [MPS] unary kernels - avoid copying tensors if they have same stride ([#148350](https://github.com/pytorch/pytorch/pull/148350))
- [MPS] Introduce strides unary op ([#148468](https://github.com/pytorch/pytorch/pull/148468))
### docs
### devs
- [MPS] Support includes in metal objects ([#145087](https://github.com/pytorch/pytorch/pull/145087))
### Untopiced
- [MPS] Fix `torch.add(x,y, alpha=2)` crash ([#143949](https://github.com/pytorch/pytorch/pull/143949))
- [MPS] Handle implicit cpu-scalar-to-gpu transfer ([#144055](https://github.com/pytorch/pytorch/pull/144055))
- Multinomial sampling fix on mps for non contiguous tensors ([#141515](https://github.com/pytorch/pytorch/pull/141515))
- [MPS] Add `aten::angle` ([#143449](https://github.com/pytorch/pytorch/pull/143449))
- Use structure binding ([#144524](https://github.com/pytorch/pytorch/pull/144524))
- [MPS][BE] Surface syntax errors shader compilation ([#144648](https://github.com/pytorch/pytorch/pull/144648))
- [MPS] fix triangular for >3D tensors ([#144545](https://github.com/pytorch/pytorch/pull/144545))
- Missing autorelease in lstm_mps caused a ton of leaked memory ([#145503](https://github.com/pytorch/pytorch/pull/145503))
- [MPS] optimize cholesky ([#145722](https://github.com/pytorch/pytorch/pull/145722))
- [MPS] Add linalg det and fix lu factor for non contiguous tensors ([#146279](https://github.com/pytorch/pytorch/pull/146279))
- [mps] Implement support for sinc() operator (inductor and eager). ([#146539](https://github.com/pytorch/pytorch/pull/146539))
- [BE] Towards MetalTensorIterator ([#146993](https://github.com/pytorch/pytorch/pull/146993))
- Add missing autoreleasepool around runUniqueGraph to prevent leaks ([#145512](https://github.com/pytorch/pytorch/pull/145512))
- [MPS] Implement and test round.decimals ([#147266](https://github.com/pytorch/pytorch/pull/147266))
- [MPS][BE] Turn `exec_unary_kernel` as MetalShaderLibrary method ([#147299](https://github.com/pytorch/pytorch/pull/147299))
- [MPS] Workaround rng bug for 5D tensors ([#147667](https://github.com/pytorch/pytorch/pull/147667))
- [MPS] Add eager support for xlog1py. ([#147687](https://github.com/pytorch/pytorch/pull/147687))
- [MPS] Add support for `entr()` in eager. ([#147948](https://github.com/pytorch/pytorch/pull/147948))
- [MPS][BE] Combine two `upsample_kernel_out_template` into one ([#148211](https://github.com/pytorch/pytorch/pull/148211))
- [MPS] Fix SDPA crash ([#148239](https://github.com/pytorch/pytorch/pull/148239))
- [MPS] Speedup interpolation ([#148277](https://github.com/pytorch/pytorch/pull/148277))
- [BE][MPS] Use `copysign` for imaginary part of sqrt ([#148286](https://github.com/pytorch/pytorch/pull/148286))
- [MPS][BE] Towards strided unary ops support ([#148449](https://github.com/pytorch/pytorch/pull/148449))
- [MPS] Fix Wreorder-init-list ([#148839](https://github.com/pytorch/pytorch/pull/148839))
- Fix invalid format string in libfmt calls ([#148855](https://github.com/pytorch/pytorch/pull/148855))
### not user facing
- Make Context to be Device-agnostic Step by Step (3/N) ([#137578](https://github.com/pytorch/pytorch/pull/137578))
- [MPS][BE] Move vectypes from Quantized to utils ([#145312](https://github.com/pytorch/pytorch/pull/145312))
- [BE][MPS] Move Gamma kernels to its own file ([#145289](https://github.com/pytorch/pytorch/pull/145289))
- [BE][MPS] Mark gamma inputs as const ([#145295](https://github.com/pytorch/pytorch/pull/145295))
- [MPS][BE] Prepare Gamma funcs to be moved ot headers ([#145309](https://github.com/pytorch/pytorch/pull/145309))
- [MPSInductor] Add `gamma` op ([#145341](https://github.com/pytorch/pytorch/pull/145341))
- [BE] [mps] Refactor UnaryConstants to be its own kernel. ([#145230](https://github.com/pytorch/pytorch/pull/145230))
- [MPS][BE] Turn `bicubic2d` into generic metal template ([#145578](https://github.com/pytorch/pytorch/pull/145578))
- [mps] Hoist erfinv logic out of the kernel in preparation for moving. ([#145568](https://github.com/pytorch/pytorch/pull/145568))
- [MPS][BE] Use conveinence methods to set args ([#145736](https://github.com/pytorch/pytorch/pull/145736))
- [MPS] Fix `c0::metal::log_gamma` correctness on M4 ([#145740](https://github.com/pytorch/pytorch/pull/145740))
- [MPS] Add `op_math_t` ([#145808](https://github.com/pytorch/pytorch/pull/145808))
- [mps] Move zeta() to special_math.h. ([#146231](https://github.com/pytorch/pytorch/pull/146231))
- [mps] Move polygamma to special_math.h. ([#146253](https://github.com/pytorch/pytorch/pull/146253))
- [BE][EZ][Metal] Mark constant inputs as constant ([#146521](https://github.com/pytorch/pytorch/pull/146521))
- [BE][EZ][Metal] Do not pass tensor length as arg ([#146522](https://github.com/pytorch/pytorch/pull/146522))
- [mps] Remove a stale comment. ([#146619](https://github.com/pytorch/pytorch/pull/146619))
- [mps] Add a shader for spherical_bessel_j0. ([#146771](https://github.com/pytorch/pytorch/pull/146771))
- [mps] Implement eager support for spherical_bessel_j0 ([#146818](https://github.com/pytorch/pytorch/pull/146818))
- [BE] Unify kernel templates instantiation ([#146965](https://github.com/pytorch/pytorch/pull/146965))
- [BE] Use `c10::multiply_integers` in cholesky_impl ([#147163](https://github.com/pytorch/pytorch/pull/147163))
- [BE][MPS]  Infer results of functor ([#147182](https://github.com/pytorch/pytorch/pull/147182))
- [MPS][BE] Add copysign integral flavors as functor ([#147183](https://github.com/pytorch/pytorch/pull/147183))
- [MPS][BE] Migrate polar to use functor ([#147184](https://github.com/pytorch/pytorch/pull/147184))
- [MPS][BE] Use stubs for floor/ceil/round/trunc ([#147286](https://github.com/pytorch/pytorch/pull/147286))
- [BE] Switch all structured funcs to stubs ([#147296](https://github.com/pytorch/pytorch/pull/147296))
- [BE] Make `exec_unary_kernel` take TensorIterator as argument ([#147297](https://github.com/pytorch/pytorch/pull/147297))
- [MPS] Add inductor support for spherical_bessel_j0. ([#147650](https://github.com/pytorch/pytorch/pull/147650))
- [MPS] Introduce a shader for `entr()`. ([#147914](https://github.com/pytorch/pytorch/pull/147914))
- [BE] Do not copy arguments in variadic template ([#147977](https://github.com/pytorch/pytorch/pull/147977))
- [MPS] Add inductor support for the `entr()` operator. ([#148128](https://github.com/pytorch/pytorch/pull/148128))
- [MPS][BE][EZ] Aggregate macros ([#148187](https://github.com/pytorch/pytorch/pull/148187))
- [BE] Remove stale arg for complex ops ([#148398](https://github.com/pytorch/pytorch/pull/148398))
- [BE] Move `sinc` kernels to the same OP family ([#148399](https://github.com/pytorch/pytorch/pull/148399))
- [MPS] Add some useful utils ([#148448](https://github.com/pytorch/pytorch/pull/148448))
- [MPS][BE] Fix `c10::metal::sinc` implementation ([#148471](https://github.com/pytorch/pytorch/pull/148471))
- [BE][MPS] Remove redundant `handle_tensor_scalar_binary_op` ([#148685](https://github.com/pytorch/pytorch/pull/148685))
### security
