
# Release Notes worksheet python_frontend

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

## python_frontend
### bc breaking

#### Change `torch.Tensor.new_tensor()` be on the given Tensor's device by default ([#144958](https://github.com/pytorch/pytorch/pull/144958))

This function was always creating the new Tensor on the "cpu" device and will now use the same device as the current Tensor object. This behavior is now consistent with other `.new_*` methods.


### deprecation
### new features
- Introduce a new `torch.utils.serialization.config` namespace for all serialization related configurations ([#143324](https://github.com/pytorch/pytorch/pull/143324))
- Add `torch.serialization.config.save.use_pinned_memory_for_d2h` to speed up `torch.save` when passed gpu devices ([#143342](https://github.com/pytorch/pytorch/pull/143342))
- Add `torch.utils.serialization.config.load.calculate_storage_offsets` to reduce random reads and significantly improve performance for storage with bad random access performance ([#143880](https://github.com/pytorch/pytorch/pull/143880))
- Add support for `__torch_function__` handler on dtype arguments, similar to subclass objects ([#145085](https://github.com/pytorch/pytorch/pull/145085))

### improvements
- Add a warning when a tensor with `requires_grad=True` is converted to a scalar ([#143261](https://github.com/pytorch/pytorch/pull/143261))
- Add support for CPU scalar in `torch.addcmul` ([#143264](https://github.com/pytorch/pytorch/pull/143264))
- Set `-DPy_LIMITED_API` flag for `py_limited_api=True` cpp_extensions ([#145764](https://github.com/pytorch/pytorch/pull/145764))
- Add support for serialization for uintx/intx in weights_only ([#147500](https://github.com/pytorch/pytorch/pull/147500))
- Add warning to `torch.jit.load` ([#143403](https://github.com/pytorch/pytorch/pull/143403))
- Make record/storage alignment in `torch.save` configurable ([#147788](https://github.com/pytorch/pytorch/pull/147788))
- Support `with` statement on torch.Stream ([#140138](https://github.com/pytorch/pytorch/pull/140138))


### bug fixes
- Fix `torch.lerp` type promotion ([#141117](https://github.com/pytorch/pytorch/pull/141117))
- Fix memory leak on `torch.Tensor` when both slots and python gc are used ([#143203](https://github.com/pytorch/pytorch/pull/143203))
- Fix `torch.bfloat16` support for `__cuda_array_interface__`. ([#143042](https://github.com/pytorch/pytorch/pull/143042))
- Fix rare dispatcher bug for inplace operations that would make the returned `torch.Tensor` incorrect. ([#145530](https://github.com/pytorch/pytorch/pull/145530))
- Stop using MKL for randomness generation on CPU ([#146174](https://github.com/pytorch/pytorch/pull/146174))
- Move accelerator detection to use build time ([#146098](https://github.com/pytorch/pytorch/pull/146098))
- Fix `torch.load` under `FakeTensorMode` to create `FakeTensor` with correct devices (for plain Tensors) ([#147786](https://github.com/pytorch/pytorch/pull/147786))
### performance
### docs
- Fix description of `input` in `torch.addbmm()` ([#146664](https://github.com/pytorch/pytorch/pull/146664))
- fix numpy docs reference ([#147697](https://github.com/pytorch/pytorch/pull/147697))
- Add `torch.cat` type promotion documentation ([#141339](https://github.com/pytorch/pytorch/pull/141339))
- Add details `torch.topk` indices stability when duplicate values ([#143736](https://github.com/pytorch/pytorch/pull/143736))
- Add overloads to `torch.diagonal` documentation ([#144214](https://github.com/pytorch/pytorch/pull/144214))
- remove incorrect warnings from `torch.{min,max}` documentation ([#146725](https://github.com/pytorch/pytorch/pull/146725))
- Update addbmm, addmm, addmv and baddbmm description ([#146689](https://github.com/pytorch/pytorch/pull/146689))
- Fix `torch.max` optional args `dim`, `keepdim` description ([#147177](https://github.com/pytorch/pytorch/pull/147177))
- Update `torch.bucketize` documentaion ([#148400](https://github.com/pytorch/pytorch/pull/148400))
- Fix docs recommending inefficient tensor op order ([#144270](https://github.com/pytorch/pytorch/pull/144270))


### devs
- Collect packages with importlib in collect_env ([#144616](https://github.com/pytorch/pytorch/pull/144616))
- added `__add__` and `__mul__` hints to `torch.Size` ([#144322](https://github.com/pytorch/pytorch/pull/144322))


### Untopiced

### not user facing
- Fix potentially undefined behaviour in index_put sample input ([#143116](https://github.com/pytorch/pytorch/pull/143116))
- Bump `nn.functional.conv3d` tolerances for `test_comprehensive` ([#135719](https://github.com/pytorch/pytorch/pull/135719))
- utils: Update md5 call to be fips compliant ([#147252](https://github.com/pytorch/pytorch/pull/147252))
- Update addr doc ([#146482](https://github.com/pytorch/pytorch/pull/146482))
- [BE] Switch `index_variable` to `torch.testing.make_tensor` ([#147892](https://github.com/pytorch/pytorch/pull/147892))
### security
