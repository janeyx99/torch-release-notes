
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
- Let `tensor_a.new_tensor()` be on `tensor_a.device` by default ([#144958](https://github.com/pytorch/pytorch/pull/144958))
### deprecation
### new features
- Expose remaining sharedMem cudaDeviceProps to python ([#143226](https://github.com/pytorch/pytorch/pull/143226))
- Add config.save.use_pinned_memory_for_d2h to serialization config ([#143342](https://github.com/pytorch/pytorch/pull/143342))
- Add support for torch function on dtype arguments ([#145085](https://github.com/pytorch/pytorch/pull/145085))
### improvements
- Refactor serialization getter/setters into torch.utils.serialization.config ([#143324](https://github.com/pytorch/pytorch/pull/143324))
- Add a warning when a tensor with requires_grad=True is converted to a scalar ([#143261](https://github.com/pytorch/pytorch/pull/143261))
- Collect packages with importlib in collect_env ([#144616](https://github.com/pytorch/pytorch/pull/144616))
- [BE] Introduce `c10::SyntaxError` ([#144647](https://github.com/pytorch/pytorch/pull/144647))
- Fix lerp weight type promotion ([#141117](https://github.com/pytorch/pytorch/pull/141117))
- Add option to serialization config to reduce random reads from get_record_offset when loading with mmap=True ([#143880](https://github.com/pytorch/pytorch/pull/143880))
- Add option to serialization config to reduce random reads from get_record_offset when loading with mmap=True ([#143880](https://github.com/pytorch/pytorch/pull/143880))
### bug fixes
- Fix Tensor clear to properly clear slots ([#143203](https://github.com/pytorch/pytorch/pull/143203))
- `__cuda_array_interface__`: Use "<V2" for bfloat16. ([#143042](https://github.com/pytorch/pytorch/pull/143042))
- Pass allow_rhs_unbacked to the stride test in metadata test too ([#143040](https://github.com/pytorch/pytorch/pull/143040))
- Work around buggy use_const_ref_for_mutable_tensors ([#145530](https://github.com/pytorch/pytorch/pull/145530))
- Temp disable MKL in  DistributionKernels.cpp ([#146174](https://github.com/pytorch/pytorch/pull/146174))
- Move get accelerator to use build time flags when possible ([#146098](https://github.com/pytorch/pytorch/pull/146098))
- Allow torch.load under FakeTensorMode to load FakeTensors with correct devices (for plain Tensors) ([#147786](https://github.com/pytorch/pytorch/pull/147786))
- Move get accelerator to use build time flags when possible ([#146098](https://github.com/pytorch/pytorch/pull/146098))
- Move get accelerator to use build time flags when possible ([#146098](https://github.com/pytorch/pytorch/pull/146098))
### performance
### docs
- [BE] Only print MKL version on x86 platforms ([#143763](https://github.com/pytorch/pytorch/pull/143763))
- [Docs] Fix description of `input` in `torch.addbmm()` ([#146664](https://github.com/pytorch/pytorch/pull/146664))
- [docs] fix numpy docs reference ([#147697](https://github.com/pytorch/pytorch/pull/147697))
### devs
### Untopiced
- Expose sharedMemPerMultiprocessor device property to python ([#143119](https://github.com/pytorch/pytorch/pull/143119))
- Add torch.cat tensors type promotion description ([#141339](https://github.com/pytorch/pytorch/pull/141339))
- Add support for CPU scalar in addcmul ([#143264](https://github.com/pytorch/pytorch/pull/143264))
- Add torch.topk indices vary description ([#143736](https://github.com/pytorch/pytorch/pull/143736))
- [Easy] Add torch.range, torch.arange params optional description ([#143731](https://github.com/pytorch/pytorch/pull/143731))
- Add get_stream_from_external API for CUDA backend ([#143799](https://github.com/pytorch/pytorch/pull/143799))
- [typing] Add type hints to `@property` and `@lazy_property` in `torch.distributions`. ([#144110](https://github.com/pytorch/pytorch/pull/144110))
- Support with statement on torch.Stream ([#140138](https://github.com/pytorch/pytorch/pull/140138))
- ReshapeTransform: added missing argument in docstring ([#144401](https://github.com/pytorch/pytorch/pull/144401))
- `torch.distributions`: replace `numbers.Number` with `torch.types.Number`. ([#145086](https://github.com/pytorch/pytorch/pull/145086))
- Set -DPy_LIMITED_API flag for py_limited_api=True extensions ([#145764](https://github.com/pytorch/pytorch/pull/145764))
- Add overloads to diagonal docs ([#144214](https://github.com/pytorch/pytorch/pull/144214))
- remove incorrect warnings from min/max documentation ([#146725](https://github.com/pytorch/pytorch/pull/146725))
- Fix torch.take_along_dim param type and default description ([#146474](https://github.com/pytorch/pytorch/pull/146474))
- Update addbmm, addmm, addmv and baddbmm description ([#146689](https://github.com/pytorch/pytorch/pull/146689))
- Fix `torch.max` optional args `dim`, `keepdim` description ([#147177](https://github.com/pytorch/pytorch/pull/147177))
- Support serialization for uintx/intx in weights_only ([#147500](https://github.com/pytorch/pytorch/pull/147500))
- Define `__all__` for `torch.utils.tensorboard` ([#147550](https://github.com/pytorch/pytorch/pull/147550))
- [Docs] update bucketize documentaion ([#148400](https://github.com/pytorch/pytorch/pull/148400))
### not user facing
- Fix potentially undefined behaviour in index_put sample input ([#143116](https://github.com/pytorch/pytorch/pull/143116))
- Bump `nn.functional.conv3d` tolerances for `test_comprehensive` ([#135719](https://github.com/pytorch/pytorch/pull/135719))
- utils: Update md5 call to be fips compliant ([#147252](https://github.com/pytorch/pytorch/pull/147252))
- Update addr doc ([#146482](https://github.com/pytorch/pytorch/pull/146482))
- [BE] Switch `index_variable` to `torch.testing.make_tensor` ([#147892](https://github.com/pytorch/pytorch/pull/147892))
### security
