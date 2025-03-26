
# Release Notes worksheet composability

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

## composability
### bc breaking
### deprecation
### new features
### improvements

**AOTDispatcher**

AOTDispatcher is a "middleware" component of torch.compile, responsible for normalizing the graph captured by dynamo
and adding training support. Some improvements:
* Fix a quadratic compile time edge case during training when you have long parallel chains of compute (#145082)
* handle compiling mutations on tangents in custom autograd.Functions (#141131)
* handle compiling buffer input mutations of the form `buffer.copy_(int)` (#141161)
* Fix handling of mutable custom operators in compile when used with `torch.inference_mode` (#147925)

** Decompositions, FakeTensor and meta tensors **

Operator decompositions, FakeTensors and meta tensors are used to trace out a graph in torch.compile and torch.export. They received several improvements:

Several operator decomps received improvements/bugfixes:
* `torch._refs.tensor` (#143461)
* `torch._refs.mean` (#147188)
* `linspace` (#147997)
* `addmv` (#143792)
New meta tensor implementations for a few pytorch operators:
* `nonzero` (#144727)
* `silu`, `sigmoid`, `_softmax`, `embedding` (#147862)
New fake tensor implementation for a few pytorch operators:
* `unique_consecutive` (#145649)
Several general FakeTensor improvements
* force `UntypedStorage.from_buffer(buf)` to return meta storage under FakeTensorMode (#146642)
* support `meta_tensor.to(device='cpu')` under `fake_mode` (#146729)

** Dynamic Shapes **

We made many improvements and bugfixes to dynamic shapes in torch.compile
* Better unbacked symint handling for `topk` (#147017)
* dynamic shape support for `interpolate(antialias=True)` backward (#141198)
* Better unbacked symint handling in the partitioner (#143877)
* Support dynamic shape inputs to `nonzer_static` (#146006)
* Improve logging in the symbolic shapes framework (provenance tracking, error messages) (#143378, #146625, #146583, #146532, #145354, #146858, #146939, #146955, #147240,#146413m  #145848, #147836, #146298)
* Simplify and speed up `_compute_symbolic_stride()` (#138844)
* Add max kwarg to `torch._check` (#144471)
* Apply hints to symbol not expr when materializing unbacked tensor intermediates in the partitioner (#144097)
* Add `backed_size_oblivious` config (#148696)
* Add `mark_unbacked` strict mode (#147333, #147342)


### bug fixes
### performance
### docs
### devs
### Untopiced


### not user facing
- remove allow-untyped-defs from torch/_prims/executor.py ([#144233](https://github.com/pytorch/pytorch/pull/144233))
- [BE]: Remove redundant contiguous copy in torch/_decomp/decompositions ([#144472](https://github.com/pytorch/pytorch/pull/144472))
- [Break XPU][Inductor UT] Fix broken XPU CI introduced by community changes ([#145058](https://github.com/pytorch/pytorch/pull/145058))
- PEP585 update - torch/_C torch/_decomp torch/_lazy torch/_library torch/_numpy torch/_prims torch/_refs torch/_strobelight ([#145102](https://github.com/pytorch/pytorch/pull/145102))
- [Break XPU] Align meta calculation for fft_r2c with _fft_r2c_mkl ([#146763](https://github.com/pytorch/pytorch/pull/146763))
- [BE][Ez]: Remove redundant empty tensor copies in meta-reg ([#147978](https://github.com/pytorch/pytorch/pull/147978))
- [BE][PYFMT] migrate PYFMT for `torch._dynamo` to `ruff format` ([#144549](https://github.com/pytorch/pytorch/pull/144549))
- [opcheck] Improve error reporting; allow atol/rtol overrides ([#146488](https://github.com/pytorch/pytorch/pull/146488))
- Back out "Fix undesired specialization on slice after split. (#142372)" ([#143356](https://github.com/pytorch/pytorch/pull/143356))
- Update decompositions_for_jvp.py ([#148821](https://github.com/pytorch/pytorch/pull/148821))
- Refactor _create_symbolic_sizes_strides_storage_offset (#138843)
- Don't overspecialize float when propagating cache guards to ShapeEnv (#145078)
- Unconditionally exclude upper bound in all size oblivious tests (#144867)
- Propagate unbacked hint when creating mod replacement (#146381)
- Introduce dynamism library (#147981)
### security
