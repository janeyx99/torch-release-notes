# Release Notes worksheet mps

The main goal of this process is to rephrase all the commit messages below to make them **clear and easy to read** by the end user. You should follow the following instructions to do so:

* **Please cleanup, and format commit titles to be readable by the general PyTorch user.** Make sure you're [following the guidance here](https://docs.google.com/document/d/14OmgGBr1w6gl1VO47GGGdwrIaUNr92DFhQbY_NEk8mQ/edit)\! Your resulting notes must be consistent and easy to read.  
* Please sort commits into the following categories (you should not rename the categories\!), I tried to pre-sort these to ease your work, feel free to move commits around if the current categorization is not good.  
* Anything that is not public facing needs to be removed.  
* If anything is miscategorized/belongs to another domain, move it to `miscategorized.md`.  
* Please scan through `miscategorized.md` and handle any commits that belong within your domain according to these instructions.  
* Please use markdown format.  
* Please use \#PR\_NUM to link to the PR, instead of `[#PR_NUM](https://github.com/pytorch/pytorch/pull/#PR_NUM)` to reduce the length of the release notes.  
* We place a lot of emphasis on the “BC-breaking” and “deprecation” sections. Those should be where the most effort goes in. The “improvements” and “bug fixes” for Python API should be nice as well.  
* Once you are finished, move this very file from `todo/` to `done/` and submit a pull request.

The categories below are as follows:

* BC breaking: All commits that are BC-breaking. These are the most important commits. If any pre-sorted commit is actually BC-breaking, do move it to this section. Each commit should contain a paragraph explaining the rational behind the change as well as an example for how to update user code [BC-Guidelines](https://docs.google.com/document/d/14OmgGBr1w6gl1VO47GGGdwrIaUNr92DFhQbY_NEk8mQ/edit#heading=h.a9htwgvvec1m).  
* Deprecations: All commits introducing deprecation. Each commit should include a small example explaining what should be done to update user code.  
* new\_features: All commits introducing a new feature (new functions, new submodule, new supported platform etc)  
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

- Prototype of torch.compile for Metal ([\#143893](https://github.com/pytorch/pytorch/pull/143893))
- Provide Metal kernel authoring via Python ([\#148972](https://github.com/pytorch/pytorch/pull/148972))

### improvements

- Adding support to MPS for operators: `angle`, `entr`, `spherical_bessel_j0`,`xlog1py`,` sinc`,`round.decimals`, `linalg.det`,` cholesky.ex`,` bilineard2d_aa`,`linalg.solve`, `zeta`, `cholesky`, `fused_rms_norm`, `lu_unpack`, `lu_factor_ex`, `slogdet` and `logdet` ([\#143449](https://github.com/pytorch/pytorch/pull/143449), [\#147948](https://github.com/pytorch/pytorch/pull/147948), [\#146818](https://github.com/pytorch/pytorch/pull/146818), [\#147687](https://github.com/pytorch/pytorch/pull/147687), [\#146539](https://github.com/pytorch/pytorch/pull/146539), [\#147266](https://github.com/pytorch/pytorch/pull/147266), [\#146279](https://github.com/pytorch/pytorch/pull/146279), [\#146799](https://github.com/pytorch/pytorch/pull/146799), [\#145526](https://github.com/pytorch/pytorch/pull/145526), [\#146531](https://github.com/pytorch/pytorch/pull/146531), [\#146465](https://github.com/pytorch/pytorch/pull/146465), [\#145701](https://github.com/pytorch/pytorch/pull/145701), [\#145301](https://github.com/pytorch/pytorch/pull/145301), [\#146681](https://github.com/pytorch/pytorch/pull/146681), [\#144651](https://github.com/pytorch/pytorch/pull/144651), [\#145341](https://github.com/pytorch/pytorch/pull/145341), [\#146771](https://github.com/pytorch/pytorch/pull/146771), [\#147914](https://github.com/pytorch/pytorch/pull/147914))
- Extending data type support for `angle` and `atan2` for long type, `torch.special.sinc` to complex, `torch.mm` / `torch.bmm` to integral types ([\#149017](https://github.com/pytorch/pytorch/pull/149017), [\#146648](https://github.com/pytorch/pytorch/pull/146648), [\#145809](https://github.com/pytorch/pytorch/pull/145809), [\#147526](https://github.com/pytorch/pytorch/pull/147526))
- Support `torch.accelerator.synchronize()` on MPS ([\#143171](https://github.com/pytorch/pytorch/pull/143171))
- Add error checking when dispatching kernel ([\#146458](https://github.com/pytorch/pytorch/pull/146458))
- For MPSInductor
  * Fix index generation for transpose ([\#143973](https://github.com/pytorch/pytorch/pull/143973))
  * Fix multi rangevar kernel invocation ([\#144050](https://github.com/pytorch/pytorch/pull/144050))
  * Better error when kernel fails to compile ([\#144649](https://github.com/pytorch/pytorch/pull/144649))
  * Fix large prod and sum reductions ([\#148975](https://github.com/pytorch/pytorch/pull/148975))
  * Adding support to MPSInductor for operators: `gamma`, `zeta`, `sinc`, `spherical_bessel_j0`, `entr` ([\#145341](https://github.com/pytorch/pytorch/pull/145341), [\#146465](https://github.com/pytorch/pytorch/pull/146465), [\#146539](https://github.com/pytorch/pytorch/pull/146539), [\#147650](https://github.com/pytorch/pytorch/pull/147650), [\#148128](https://github.com/pytorch/pytorch/pull/148128))

### bug fixes

- Workaround for `gather_out` in MPS backend ([\#135543](https://github.com/pytorch/pytorch/pull/135543))
- Fix fmin/fmax for scalar argument ([\#143934](https://github.com/pytorch/pytorch/pull/143934))
- Fix crash when mm is invoked with mixed dtypes ([\#143948](https://github.com/pytorch/pytorch/pull/143948))
- Fix `torch.add(x,y, alpha=2)` crash ([\#143949](https://github.com/pytorch/pytorch/pull/143949))
- Fix `nllnd_loss_backward` crash with different dtypes ([\#144170](https://github.com/pytorch/pytorch/pull/144170))
- Make sure that MPSStream is usable from C++ ([\#144559](https://github.com/pytorch/pytorch/pull/144559))
- Make MPSProfiler usable from C++ ([\#144560](https://github.com/pytorch/pytorch/pull/144560))
- Fix regression in con-contiguous bitwise ops ([\#146085](https://github.com/pytorch/pytorch/pull/146085))
- Fix lu factor for large tensors with bs\>1 ([\#146753](https://github.com/pytorch/pytorch/pull/146753))
- Ensure 4d input in `_scaled_dot_product_attention_math_mps` ([\#146623](https://github.com/pytorch/pytorch/pull/146623))
- Fix `cholesky_ex` for empty inputs ([\#147159](https://github.com/pytorch/pytorch/pull/147159))
- Fix attention for \>4d tensors ([\#147545](https://github.com/pytorch/pytorch/pull/147545))
- Fix empty placeholder error for smooth l1 loss ([\#148133](https://github.com/pytorch/pytorch/pull/148133))
- Fix sqrt and other for `torch.chalf` ([\#148285](https://github.com/pytorch/pytorch/pull/148285))
- Fix `unary_kernel_strided` logic ([\#148512](https://github.com/pytorch/pytorch/pull/148512))
- Fix scalar to tensors bitshifts ([\#148686](https://github.com/pytorch/pytorch/pull/148686))
- Fix multinomial sampling for non-contiguous tensors ([\#141515](https://github.com/pytorch/pytorch/pull/141515))
- Fix triangular for \>3D tensors ([\#144545](https://github.com/pytorch/pytorch/pull/144545))
- Fix missing autorelease in `lstm_mps` causing leaked memory ([\#145503](https://github.com/pytorch/pytorch/pull/145503))
- Fix missing autoreleasepool around runUniqueGraph to prevent leaks ([\#145512](https://github.com/pytorch/pytorch/pull/145512))
- Workaround rng bug for 5D tensors ([\#147667](https://github.com/pytorch/pytorch/pull/147667))
- Fix Wreorder-init-list ([\#148839](https://github.com/pytorch/pytorch/pull/148839))
- Fix invalid format string in libfmt calls ([\#148855](https://github.com/pytorch/pytorch/pull/148855))
- Fix `c10::metal::log_gamma` correctness on M4 ([\#145740](https://github.com/pytorch/pytorch/pull/145740))
- Fix lu factor for non contiguous tensors ([\#146279](https://github.com/pytorch/pytorch/pull/146279))
- In MPSInductor:
  * Fix `min`/`max` reductions over large dims ([\#149004](https://github.com/pytorch/pytorch/pull/149004))
  * Fix argmin/max signatures ([\#149020](https://github.com/pytorch/pytorch/pull/149020))
  * Fix `masked`/`where` for inf values ([\#144500](https://github.com/pytorch/pytorch/pull/144500))

### performance

- Faster integer batched matmul ([\#147877](https://github.com/pytorch/pytorch/pull/147877))
- Implement linear1d as shader ([\#148154](https://github.com/pytorch/pytorch/pull/148154))
- Metal unary kernel for sqrt ([\#148272](https://github.com/pytorch/pytorch/pull/148272))
- Faster unary operations for strided tensors ([\#148350](https://github.com/pytorch/pytorch/pull/148350))
- Introduce strides unary op ([\#148468](https://github.com/pytorch/pytorch/pull/148468))
- Implemented `masked_fill_scalar` as shader ([\#147369](https://github.com/pytorch/pytorch/pull/147369))
- Implement `bilineard2d` as shader ([\#145581](https://github.com/pytorch/pytorch/pull/145581))
- Optimize Cholesky ([\#145722](https://github.com/pytorch/pytorch/pull/145722))
- Speedup interpolation ([\#148277](https://github.com/pytorch/pytorch/pull/148277))

### docs

### devs

- Support includes in metal objects ([\#145087](https://github.com/pytorch/pytorch/pull/145087))
- Context manager to capture Metal commands for debugging/profiling ([\#144561](https://github.com/pytorch/pytorch/pull/144561))

### Untopiced

### not user facing

- Make Context to be Device-agnostic Step by Step ([\#137578](https://github.com/pytorch/pytorch/pull/137578))
- Move vectypes from Quantized to utils ([\#145312](https://github.com/pytorch/pytorch/pull/145312))
- Move Gamma kernels to its own file ([\#145289](https://github.com/pytorch/pytorch/pull/145289))
- Mark gamma inputs as const ([\#145295](https://github.com/pytorch/pytorch/pull/145295))
- Prepare Gamma funcs to be moved to headers ([\#145309](https://github.com/pytorch/pytorch/pull/145309))
- Refactor UnaryConstants to be its own kernel. ([\#145230](https://github.com/pytorch/pytorch/pull/145230))
- Turn `bicubic2d` into generic metal template ([\#145578](https://github.com/pytorch/pytorch/pull/145578))
- Hoist erfinv logic out of the kernel in preparation for moving. ([\#145568](https://github.com/pytorch/pytorch/pull/145568))
- Use convenience methods to set args ([\#145736](https://github.com/pytorch/pytorch/pull/145736))
- Add `op_math_t` ([\#145808](https://github.com/pytorch/pytorch/pull/145808))
- Move zeta() and polygamma() to special\_math.h. ([\#146231](https://github.com/pytorch/pytorch/pull/146231), [\#146253](https://github.com/pytorch/pytorch/pull/146253))
- Mark constant inputs as constant ([\#146521](https://github.com/pytorch/pytorch/pull/146521))
- Do not pass tensor length as arg ([\#146522](https://github.com/pytorch/pytorch/pull/146522))
- Remove a stale comment. ([\#146619](https://github.com/pytorch/pytorch/pull/146619))
- Unify kernel templates instantiation ([\#146965](https://github.com/pytorch/pytorch/pull/146965))
- Use `c10::multiply_integers` in cholesky\_impl ([\#147163](https://github.com/pytorch/pytorch/pull/147163))
- Infer results of functor ([\#147182](https://github.com/pytorch/pytorch/pull/147182))
- Add copysign integral flavors as functor ([\#147183](https://github.com/pytorch/pytorch/pull/147183))
- Migrate polar to use functor ([\#147184](https://github.com/pytorch/pytorch/pull/147184))
- Use stubs for floor/ceil/round/trunc ([\#147286](https://github.com/pytorch/pytorch/pull/147286))
- Switch all structured funcs to stubs ([\#147296](https://github.com/pytorch/pytorch/pull/147296))
- Make `exec_unary_kernel` take TensorIterator as argument ([\#147297](https://github.com/pytorch/pytorch/pull/147297))
- Turn `exec_unary_kernel` as MetalShaderLibrary method ([\#147299](https://github.com/pytorch/pytorch/pull/147299))
- Do not copy arguments in variadic template ([\#147977](https://github.com/pytorch/pytorch/pull/147977))
- Aggregate macros ([\#148187](https://github.com/pytorch/pytorch/pull/148187))
- Remove stale arg for complex ops ([\#148398](https://github.com/pytorch/pytorch/pull/148398))
- Move `sinc` kernels to the same OP family ([\#148399](https://github.com/pytorch/pytorch/pull/148399))
- Add some useful utils ([\#148448](https://github.com/pytorch/pytorch/pull/148448))
- Fix `c10::metal::sinc` implementation ([\#148471](https://github.com/pytorch/pytorch/pull/148471))
- Remove redundant `handle_tensor_scalar_binary_op` ([\#148685](https://github.com/pytorch/pytorch/pull/148685))
- Handle implicit cpu-scalar-to-gpu transfer ([\#144055](https://github.com/pytorch/pytorch/pull/144055))
- Towards MetalTensorIterator ([\#146993](https://github.com/pytorch/pytorch/pull/146993), [\#147023](https://github.com/pytorch/pytorch/pull/147023))
- Turn nextafter into functor ([\#147018](https://github.com/pytorch/pytorch/pull/147018))
- Align bitshift behavior with CPU ([\#148719](https://github.com/pytorch/pytorch/pull/148719))
- Surface syntax errors shader compilation ([\#144648](https://github.com/pytorch/pytorch/pull/144648))
- Combine two `upsample_kernel_out_template` into one ([\#148211](https://github.com/pytorch/pytorch/pull/148211))
- Use `copysign` for imaginary part of sqrt ([\#148286](https://github.com/pytorch/pytorch/pull/148286))
- Towards strided unary ops support ([\#148449](https://github.com/pytorch/pytorch/pull/148449))

### security

