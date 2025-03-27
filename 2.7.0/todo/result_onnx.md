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

#### `torch.onnx.dynamo_export` now uses the ExportedProgram logic path (#137296)

Users using the `torch.onnx.dynamo_export` API may see some `ExportOptions` become
unsupported due to an internal switch to use `torch.onnx.export(..., dynamo=True)`: `diagnostic_options`, `fake_context` and `onnx_registry` are removed/ignored by `ExportOptions`. Only `dynamic_shapes` is retained.

Users should move to use the `dynamo=True` option on `torch.onnx.export` as
`torch.onnx.dynamo_export` is now deprecated. Leverage the [`dynamic_shapes`](https://pytorch.org/docs/stable/export.html#torch.export.export) argument in `torch.onnx.export` for specifying dynamic shapes on the model.

Version 2.6

```py
torch.onnx.dynamo_export(model, *args, **kwargs)
```

Version 2.7

```py
torch.onnx.export(model, args, kwargs=kwargs, dynamo=True)
```

### deprecation

#### `torch.onnx.dynamo_export` is deprecated (#146425, #146639, #146923)

Users should use the `dynamo=True` option on `torch.onnx.export`.

Version 2.6

```py
torch.onnx.dynamo_export(model, *args, **kwargs)
```

Version 2.7

```py
torch.onnx.export(model, args, kwargs=kwargs, dynamo=True)
```

### new features

### `torch.onnx.verification.verify_onnx_program` (#148396, #148706, #148730, #148707)

A new verification API `torch.onnx.verification.verify_onnx_program` can now be used to verify numerical accuracy of the exported ONNX model. Users can use the `compare_intermediates` option to identify any operator that causes numerical discrepancies in intermediate tensors. It is possible to use a tool like [model-explorer](https://github.com/justinchuby/model-explorer-onnx) to visualize the verification results.

- Support custom axis name through `dynamic_shapes` (#146321)
- `torch.onnx.export(dynamo=True)` now optimizes the output model by default (#146187)

### improvements

- Dynamic shapes support is improved (#144801)
- Automatically convert `dynamic_axes` to `dynamic_shapes` with `torch.export.Dim.AUTO` (#143158)
- Fix bug for exporting `torch.cdist` into onnx and support 'compute_mode' (#144213)
- Remove `LegacyDynamoStrategy` (#145442)
- Set warning stacklevel so it appears at the `torch.onnx` call site (#147165)
- Pick up missing types in `dynamic_shapes` renaming (#147407)
- Update saved exported program in debugging report if the exporting passes `run_decomposition()` (#148617)
- Use `torch export` to get `dynamic_shapes` for JIT convert strategy (#148627)
- Use `torch.export.Dim.AUTO` in `dynamo_export` (#144356)
- Support complex comparison when `verify=True` (#148619)

### bug fixes

- Support subgraphs with 1+ outputs (#145860)
- Delete `rename_dynamic_shapes_with_model_inputs` (#146002)
- Handle number of outputs in builder (#147164)
- Fix missed None type support in `dynamic_shapes` string cases (#148025)

### performance

### docs

- Update TorchDynamo-based ONNX Exporter memory usage example code. (#144139)
- Deprecation message follow up (#147005)

### devs

### Untopiced

### not user facing

- Fix a misspelling (#143301)
- remove allow-untyped-defs from `onnx/_internal/_lazy_import.py` (#143943)
- remove allow-untyped-defs from `torch/onnx/operators.py` (#144133)
- remove allow-untyped-defs `onnx/_internal/exporter/_fx_passes.py` (#144134)
- PEP585 update - `torch/onnx` (#145174)
- Migrate `test_torch_export_with_onnxruntime.py` to `test_small_models_e2e.py` (#146095)
- Apply ruff fixes to tests (#146140)
- PEP585: Add noqa to necessary tests (#146391)
- Remove reference to `onnxscript` rewriter (#147003)
- Consolidate constants to a single location (#147166)
- Move and improve error reproduction logic in test (#147391)
- Refactor dispatcher and registry (#147396)
- Add scaffolding for onnx decomp and logic for op tests (#147392)
- Update ruff linter for PEP585 (#147540)
- Use `onnxscript` apis for 2.7 (#148453)
- Assert capture strategy in tests (#148348)
- Remove inaccurate test comment (#148813)

### security
