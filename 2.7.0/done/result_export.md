
# Release Notes worksheet export

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

## export
### bc breaking
### deprecation
### new features
### improvements
#### serialization
- Add float8 support in serialization schema (#143343)
- Allow pickle protocol overriding for serialization (#142253)
- Add serialization support for SymInt inputs in higher-order op subgraphs (#142284)
- Unify single-output and multi-output serialization schemas for higher-order op subgraphs (#143227)
- Add `"+export"` logging to de/serialization process (#145283)
- Sync model container types to serialization schema (#145959)
- Serialize pytree namedtuple field names in input spec (#145956)
- Replace `builtins.getattr` with serializable higher-order-op for tensor subclasses (#145772)
#### dynamic shapes
- Support slice operations with SymInt indices in non-strict export (#143217)
- Export with automatic dynamic shapes (`Dim.AUTO`) for TorchScript -> Export Converter (#138273)
- Support partially specifying dimensions in `ShapesCollection` (#147534)
#### draft export
- Report frequency of data-dependent errors in draft export (#145030)
- Report LOC for data-dependent errors in draft export (#145443)
- Add tlparse for draft export (#145810)
- Deduplicate `expression_created` logging in draft export (#146859)
- Remove `report` as return output for draft export, attached as `ep._report` (#147558)
#### miscellaneous
- Don't decompose custom triton ops when exporting (#144284)
- Handle input/buffer mutations for joint-graph export (#144806)
- Allow `builtin` bitshift ops in verifier (#145802)
- Introduce `aoti_call_delegate` higher-order-op for eager-mode runnability (#145630)
- Include tensor subclass buffers in parametrization rules (#145991)
- Expose pytree namedtuple metadata to `FlatArgsAdapter` (#146107)
- Implement OSS-only model runner (#146440)
- Exclude core ATen ops `upsample_bilinear2d.vec`, `nearest2d.vec` from default decomposition table (#147153)
- Improve error message for unsupported input types (#147532)
- Initial support for exporting methods (#147573)
### bug fixes
#### serialization
- Rewrite the export schema format to archive without BC-breakage (#142511)
- Serialize all dataclass fields, including default-valued members, in export schema (#142286)
- Fix SymBool incorrectly serialized as bools (#144295)
- Fix serialization roundtrippability for nodes with default arguments (#144686)
- Fix deserializing bool graph outputs (#144791)
- Fix deserialization for `and_` operator (#145506)
- Explicitly serialize `unbacked_bindings` (#144894)
- Relax serialization assertion to warning for `unbacked_bindings` keys (#145777)
- Avoid always printing GraphModule in de/serialization logging (#145857)
- Bump ShapeEnv unbacked symbol counters for `unbacked_bindings` in deserialization (#145882)
- Fix serialization for nested terms in `nn_module_stack` (#145901)
- Fix typo in SymFloat serialization (#146112)
- Fix deserialization for `.requires_grad` field (#146351)
- Support `math.trunc` ops for serialization (#146715)
- Serialize `math.inf` and `NaN` as strings (#146490)
- Loosen SymInt input serialization for Inductor (#147237)
#### draft export
- Fix dense-in-memory check for fake-kernel inference, for draft export (#145653)
- Fix `lazy_trace_handler` bug in draft export logging (#146106)
- Only clear pending unbacked symbols for overwritten fake-kernels for draft export (#147427)
- Ignore when real-tensor fallback fails in draft export (#147779)
#### miscellaneous
- Fix dynamic shape constraint checking when non-strict retracing (#143442)
- Fix `._modules` corner case for `nn_module_stack` metadata in strict-mode (#142823)
- Fix placeholder name ordering for kwargs in non-strict mode (#144278)
- Extend support for distributed ops (`all_reduce`, `all_gather`, `all_gather_into_tensor`, `all_to_all_single`, `reduce_scatter_tensor`) in non-strict mode (#147133, #147417)
- Fix error with unflattener submodule reordering (#146181)
- Make `stack_trace` field optional in `insert_custom_op_guards` pass (#146438)
- Differentiate `ScriptModules` and `ScriptObjects` for TorchBind (#147399)
- Restore lost input mutations with `export_tracepoint` (#148709)
### performance
### docs
- [Export Programming Model](https://pytorch.org/docs/main/export.programming_model.html): #143546
- Update dynamic shapes docs for `dims()` and suggested fixes parser: #142510
- Clean up docstring for `torch.export.load()`: #141490
### devs
### Untopiced
### not user facing
- Forward fix D67044185 (#143193)
- Fix retraceability tests for graph modules by not preserving `module_call_signatures` ([#143676]
- Improve typing inference with TypeIs (#144682)
- Tweak `schema_check` to handle annotated builtin types (#145154)
- Clean up local imports from export (#145287)
- Fix generated header file in `schema_check.py` (#146208)
- Update `tag_` for union setters in `schema_check.py` (#146912)
- Generate printers/parsers functions for serialization enum values (#147126)
- Store self in unflattened module metadata (#147238)
- Reland OSS model runner (#147535)
- Sync aoti schema to schema.py (#148017)
- Fix "invalid application of 'sizeof' to an incomplete type" (#148854)
- remove allow-untyped-defs for torch/_export/passes/remove_runtime_assertions.py ([#143435](https://github.com/pytorch/pytorch/pull/143435))
- remove allow-untyped-defs from _export/pass_infra/proxy_value.py ([#143944](https://github.com/pytorch/pytorch/pull/143944))
- remove allow-untyped-defs from _export/db/logging.py ([#144093](https://github.com/pytorch/pytorch/pull/144093))
- remove allow-untyped-defs from export/_remove_auto_functionalized_pass.py ([#144135](https://github.com/pytorch/pytorch/pull/144135))
- Migrate from Tuple -> tuple in torch/_export ([#144262](https://github.com/pytorch/pytorch/pull/144262))
- remove allow-untyped-defs from torch/export/_remove_auto_functionalized_pass.py ([#144230](https://github.com/pytorch/pytorch/pull/144230))
- Support getattr for tensor subclasses in pre-dispatch export via patching tensor.getattr ([#143946](https://github.com/pytorch/pytorch/pull/143946))
- PEP585 update - torch/_export ([#145138](https://github.com/pytorch/pytorch/pull/145138))
- PEP585 update - torch/export ([#145165](https://github.com/pytorch/pytorch/pull/145165))
- [BE]: Enable ruff SLOT checks ([#146276](https://github.com/pytorch/pytorch/pull/146276))
### security
