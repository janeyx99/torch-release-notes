
# Release Notes worksheet jit

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

## jit
### bc breaking
### deprecation
### new features
### improvements
- Relax type-checks for empty dicts (#147167)


### bug fixes
### performance
### docs
### devs
### Untopiced


### not user facing
- remove allow-untyped-defs for torch/jit/_ir_utils.py ([#143366](https://github.com/pytorch/pytorch/pull/143366))
- clean up type nits on torch/jit/_ir_utils.py ([#143371](https://github.com/pytorch/pytorch/pull/143371))
- remove allow-untyped-defs from torch/jit/_passes/_property_propagation.py ([#144132](https://github.com/pytorch/pytorch/pull/144132))
- remove allow-untyped-defs from torch/jit/_pickle.py ([#144625](https://github.com/pytorch/pytorch/pull/144625))
- PEP585 update - mostly toplevels ([#145178](https://github.com/pytorch/pytorch/pull/145178))
- [NFC] Fix some minor typos. ([#145599](https://github.com/pytorch/pytorch/pull/145599))
- [ROCm][Windows] Remove external linkage from an anonymous namespace ([#146607](https://github.com/pytorch/pytorch/pull/146607))
- [ROCm][Windows] Fix clang-cl error related to -Wmissing prototypes enabled ([#146981](https://github.com/pytorch/pytorch/pull/146981))
- scriptfunction: Make sure we have valid __name__ and __qualname__ ([#147906](https://github.com/pytorch/pytorch/pull/147906))
- Fix floating point literals in IRPrinter ([#142119](https://github.com/pytorch/pytorch/pull/142119))
- [ODML] Make the ML feature provider thread safe ([#143418](https://github.com/pytorch/pytorch/pull/143418))
- Remove unneeded std::make_optional ([#143575](https://github.com/pytorch/pytorch/pull/143575))
- Apply Ruff fixes and pyupgrade to torch/jit ([#144208](https://github.com/pytorch/pytorch/pull/144208))
- Expose ToIValueAllowNumbersAsTensors to TORCH_PYTHON_API so we can use it in monarch ([#146087](https://github.com/pytorch/pytorch/pull/146087))
- [torch] fix exception types in custom class magic setattr/getattr ([#146516](https://github.com/pytorch/pytorch/pull/146516))
- [torch] fix builds for older pybind ([#146630](https://github.com/pytorch/pytorch/pull/146630))
- [StaticRuntime] Fix a bug that memory planner ignores subblocks (#146728) ([#146855](https://github.com/pytorch/pytorch/pull/146855))
- [StaticRuntime] Support a new pattern for ClipRangesToGatherToOffsets ([#146931](https://github.com/pytorch/pytorch/pull/146931))
- Fix clang-tidy warnings in torch/jit ([#146963](https://github.com/pytorch/pytorch/pull/146963))
- [StaticRuntime] Support a new pattern (aten::to with 5 inputs) for ClipRangesToGatherToOffsets ([#147189](https://github.com/pytorch/pytorch/pull/147189))
- Turn onnx functions into static ([#147598](https://github.com/pytorch/pytorch/pull/147598))
- Fix crash in -[PTMCoreMLCompiler _compileModel:atPath:] ([#147809](https://github.com/pytorch/pytorch/pull/147809))
- Suppress build warnings when gcc-11 is used ([#148763](https://github.com/pytorch/pytorch/pull/148763))


### security
