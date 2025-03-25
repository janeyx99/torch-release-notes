# Release Notes worksheet onnx

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

## onnx

### bc breaking

- [ONNX] Set USE_EXPERIMENTAL_LOGIC to True ([#137296](https://github.com/pytorch/pytorch/pull/137296))

### deprecation

- [ONNX] Create deprecation warning on dynamo_export ([#146003](https://github.com/pytorch/pytorch/pull/146003))
- [ONNX] Create deprecation warning on dynamo_export ([#146425](https://github.com/pytorch/pytorch/pull/146425))
- [ONNX] Adjust and add deprecation messages ([#146639](https://github.com/pytorch/pytorch/pull/146639))

### new features

- [ONNX] Support custom axis name through dynamic_shapes ([#146321](https://github.com/pytorch/pytorch/pull/146321))

### improvements

- [ONNX] Automatically convert dynamic_axes to dynamic_shapes with torch.export.Dim.AUTO ([#143158](https://github.com/pytorch/pytorch/pull/143158))
- [onnx] Fix bug for exporting torch.cdist into onnx and support 'compute_mode' ([#144213](https://github.com/pytorch/pytorch/pull/144213))
- [ONNX] Use python_dispatcher in type promotion ([#144801](https://github.com/pytorch/pytorch/pull/144801))
- [ONNX] Remove LegacyDynamoStrategy ([#145442](https://github.com/pytorch/pytorch/pull/145442))
- [ONNX] torch.onnx.export(dynamo=True) changes optimization to default ([#146187](https://github.com/pytorch/pytorch/pull/146187))
- [ONNX] Set warning stacklevel so it appears at the torch.onnx call site ([#147165](https://github.com/pytorch/pytorch/pull/147165))
- [ONNX] Pick up missing types in dynamic shapes renaming ([#147407](https://github.com/pytorch/pytorch/pull/147407))
- [ONNX] Update saved exported program in debugging report if the exporting passes run_decomposition() ([#148617](https://github.com/pytorch/pytorch/pull/148617))
- [ONNX] Use torch export to get dynamic shapes for JIT convert strategy ([#148627](https://github.com/pytorch/pytorch/pull/148627))
- [ONNX] Use torch.export.Dim.AUTO in dynamo_export ([#144356](https://github.com/pytorch/pytorch/pull/144356))
- [ONNX] Support complex comparison when verify=True ([#148619](https://github.com/pytorch/pytorch/pull/148619))

### bug fixes

- [ONNX] Support subgraphs with 1+ outputs ([#145860](https://github.com/pytorch/pytorch/pull/145860))
- [ONNX] Delete `rename_dynamic_shapes_with_model_inputs` ([#146002](https://github.com/pytorch/pytorch/pull/146002))
- [ONNX] Handle number of outputs in builder ([#147164](https://github.com/pytorch/pytorch/pull/147164))
- [ONNX] Fix missed None type support in dyamic shapes string cases ([#148025](https://github.com/pytorch/pytorch/pull/148025))

### performance

### docs

- Update TorchDynamo-based ONNX Exporter memory usage example code. ([#144139](https://github.com/pytorch/pytorch/pull/144139))
- [ONNX] Deprecation message follow up ([#147005](https://github.com/pytorch/pytorch/pull/147005))

### devs

### Untopiced

- [ONNX][reland2] Create deprecation warning on dynamo_export ([#146923](https://github.com/pytorch/pytorch/pull/146923))

### not user facing

- Fix a misspelling [ONNX] ([#143301](https://github.com/pytorch/pytorch/pull/143301))
- remove allow-untyped-defs from onnx/_internal/_lazy_import.py ([#143943](https://github.com/pytorch/pytorch/pull/143943))
- remove allow-untyped-defs from torch/onnx/operators.py ([#144133](https://github.com/pytorch/pytorch/pull/144133))
- remove allow-untyped-defs onnx/_internal/exporter/_fx_passes.py ([#144134](https://github.com/pytorch/pytorch/pull/144134))
- PEP585 update - torch/onnx ([#145174](https://github.com/pytorch/pytorch/pull/145174))
- [ONNX] Migrate test_torch_export_with_onnxruntime.py to test_small_models_e2e.py ([#146095](https://github.com/pytorch/pytorch/pull/146095))
- Apply ruff fixes to tests ([#146140](https://github.com/pytorch/pytorch/pull/146140))
- PEP585: Add noqa to necessary tests ([#146391](https://github.com/pytorch/pytorch/pull/146391))
- [ONNX][dort] Remove reference to onnxscript rewriter ([#147003](https://github.com/pytorch/pytorch/pull/147003))
- [ONNX] Consolidate constants to a single location ([#147166](https://github.com/pytorch/pytorch/pull/147166))
- [ONNX] Move and improve error reproduction logic in test ([#147391](https://github.com/pytorch/pytorch/pull/147391))
- [ONNX] Refactor dispatcher and registry ([#147396](https://github.com/pytorch/pytorch/pull/147396))
- [ONNX] Add scaffolding for onnx decomp and logic for op tests ([#147392](https://github.com/pytorch/pytorch/pull/147392))
- Update ruff linter for PEP585 ([#147540](https://github.com/pytorch/pytorch/pull/147540))
- [BE][CI][Easy] bump `ruff` to 0.9.0: long statements in docstrings ([#146509](https://github.com/pytorch/pytorch/pull/146509))
- [ONNX] Create VerificationInterpreter ([#148396](https://github.com/pytorch/pytorch/pull/148396))
- [ONNX] Use onnxscript apis for 2.7 ([#148453](https://github.com/pytorch/pytorch/pull/148453))
- [ONNX] Assert capture strategy in tests ([#148348](https://github.com/pytorch/pytorch/pull/148348))
- [ONNX] Improve verify_onnx_program to use VerificationInterpreter ([#148706](https://github.com/pytorch/pytorch/pull/148706))
- [ONNX] Handle error in verification interpreter ([#148730](https://github.com/pytorch/pytorch/pull/148730))
- [ONNX] Remove inaccurate test comment ([#148813](https://github.com/pytorch/pytorch/pull/148813))

### security
