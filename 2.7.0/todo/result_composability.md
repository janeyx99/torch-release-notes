
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
### bug fixes
### performance
### docs
### devs
### Untopiced
- Register nonzero for meta device for FBLSim ([#144727](https://github.com/pytorch/pytorch/pull/144727))
- support numbers as tensors for aten.copy(Tensor, Tensor) ([#141161](https://github.com/pytorch/pytorch/pull/141161))
- dynamic shape support for interpolate(antialias=True) backward ([#141198](https://github.com/pytorch/pytorch/pull/141198))
- partitioner: avoid inserting duplicates into heap ([#145082](https://github.com/pytorch/pytorch/pull/145082))
- dont assign a size to _assert_scalar in partitioner ([#143877](https://github.com/pytorch/pytorch/pull/143877))
- Add fake_impl for unique_consecutive ([#145649](https://github.com/pytorch/pytorch/pull/145649))
- [opcheck] Improve error reporting; allow atol/rtol overrides ([#146488](https://github.com/pytorch/pytorch/pull/146488))
- Re-add stft option to align window for center = false ([#146379](https://github.com/pytorch/pytorch/pull/146379))
- [poc] force UntypedStorage.from_buffer(buf) to return meta storage under FakeTensorMode ([#146642](https://github.com/pytorch/pytorch/pull/146642))
- support meta_tensor.to(device='cpu') under fake_mode ([#146729](https://github.com/pytorch/pytorch/pull/146729))
- support input mutations on tangents in compile ([#141131](https://github.com/pytorch/pytorch/pull/141131))
- Fix auto_functionalize x inference_mode ([#147925](https://github.com/pytorch/pytorch/pull/147925))
### not user facing
- Back out "Fix undesired specialization on slice after split. (#142372)" ([#143356](https://github.com/pytorch/pytorch/pull/143356))
- remove allow-untyped-defs from torch/_prims/executor.py ([#144233](https://github.com/pytorch/pytorch/pull/144233))
- Fix torch._refs.tensor error with empty list ([#143461](https://github.com/pytorch/pytorch/pull/143461))
- Remove extra copy torch/_prims ([#144407](https://github.com/pytorch/pytorch/pull/144407))
- [BE]: Remove redundant contiguous copy in torch/_decomp/decompositions ([#144472](https://github.com/pytorch/pytorch/pull/144472))
- [Break XPU][Inductor UT] Fix broken XPU CI introduced by community changes ([#145058](https://github.com/pytorch/pytorch/pull/145058))
- PEP585 update - torch/_C torch/_decomp torch/_lazy torch/_library torch/_numpy torch/_prims torch/_refs torch/_strobelight ([#145102](https://github.com/pytorch/pytorch/pull/145102))
- nonzero_static with symint size ([#146006](https://github.com/pytorch/pytorch/pull/146006))
- Fix meta impl for topk ([#147017](https://github.com/pytorch/pytorch/pull/147017))
- [Break XPU] Align meta calculation for fft_r2c with _fft_r2c_mkl ([#146763](https://github.com/pytorch/pytorch/pull/146763))
- Fix torch.mean out dtype check ([#147188](https://github.com/pytorch/pytorch/pull/147188))
- [BE][Ez]: Remove redundant empty tensor copies in meta-reg ([#147978](https://github.com/pytorch/pytorch/pull/147978))
- [BE][PYFMT] migrate PYFMT for `torch._dynamo` to `ruff format` ([#144549](https://github.com/pytorch/pytorch/pull/144549))
- Add some more meta kernels ([#147862](https://github.com/pytorch/pytorch/pull/147862))
- Fix decomp for linspace ([#147997](https://github.com/pytorch/pytorch/pull/147997))
- Fix empty matrix handling of addmv in inductor ([#143792](https://github.com/pytorch/pytorch/pull/143792))
- Update decompositions_for_jvp.py ([#148821](https://github.com/pytorch/pytorch/pull/148821))
### security
