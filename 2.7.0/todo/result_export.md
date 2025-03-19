
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
### bug fixes
### performance
### docs
### devs
### Untopiced
- fix dynamo nn module stack fqn ([#142823](https://github.com/pytorch/pytorch/pull/142823))
- [sigmoid] Write the new export schema format to archive without breaking compatibility. ([#142511](https://github.com/pytorch/pytorch/pull/142511))
- [export][ez] Fix forward D67044185 ([#143193](https://github.com/pytorch/pytorch/pull/143193))
- support slicing with symints in non-strict ([#143217](https://github.com/pytorch/pytorch/pull/143217))
- [export] Serialize all dataclass fields ([#142286](https://github.com/pytorch/pytorch/pull/142286))
- Add float8 support in serde schema ([#143343](https://github.com/pytorch/pytorch/pull/143343))
- fix checking non-trivial input constraints ([#143442](https://github.com/pytorch/pytorch/pull/143442))
- torch export programming model ([#143546](https://github.com/pytorch/pytorch/pull/143546))
- [ts converter] use Dim.AUTO for ts -> export converter ([#138273](https://github.com/pytorch/pytorch/pull/138273))
- graph module retracing without preserving MCS ([#143676](https://github.com/pytorch/pytorch/pull/143676))
- [docs/export] update dynamic_shapes docs ([#142510](https://github.com/pytorch/pytorch/pull/142510))
- fix non-strict placeholder naming with kwargs ([#144278](https://github.com/pytorch/pytorch/pull/144278))
- [BE] Clean up ExecuTorch Export Docstring ([#141490](https://github.com/pytorch/pytorch/pull/141490))
- [export] Fix sym_bool serialization ([#144295](https://github.com/pytorch/pytorch/pull/144295))
- [export] Add pickle protocol ([#142253](https://github.com/pytorch/pytorch/pull/142253))
- [export] Add support for serializing symint inputs ([#142284](https://github.com/pytorch/pytorch/pull/142284))
- [reland][export] don't decompose custom triton op when exporting ([#144284](https://github.com/pytorch/pytorch/pull/144284))
- [export] Unify single and multiple return for hops ([#143227](https://github.com/pytorch/pytorch/pull/143227))
- [BE]: Improve typing inference with TypeIs ([#144682](https://github.com/pytorch/pytorch/pull/144682))
- [export] Load side info about pos/kw argument kind for serialization. ([#144686](https://github.com/pytorch/pytorch/pull/144686))
- fix as_bool serde ([#144791](https://github.com/pytorch/pytorch/pull/144791))
- [export] handle buffer/input mutations for joint-graph ([#144806](https://github.com/pytorch/pytorch/pull/144806))
- Tweak schema_check to handle annotated builtin types ([#145154](https://github.com/pytorch/pytorch/pull/145154))
- [export][be] Clean up local imports from export [1/n] ([#145287](https://github.com/pytorch/pytorch/pull/145287))
- [BE][export] add "+export" logging to de/serialization ([#145283](https://github.com/pytorch/pytorch/pull/145283))
- [draft export] count how many times a data-dep error shows up ([#145030](https://github.com/pytorch/pytorch/pull/145030))
- serde and_ operator ([#145506](https://github.com/pytorch/pytorch/pull/145506))
- serde unbacked bindings ([#144894](https://github.com/pytorch/pytorch/pull/144894))
- [draft_export] fix dense-in-memory check for inferring fakes ([#145653](https://github.com/pytorch/pytorch/pull/145653))
- relax assertion to warning for unbacked binding names ([#145777](https://github.com/pytorch/pytorch/pull/145777))
- [draft_export] add LOC for data-dep error logging ([#145443](https://github.com/pytorch/pytorch/pull/145443))
- [export] don't always print GM in serdes logging ([#145857](https://github.com/pytorch/pytorch/pull/145857))
- [export] allow bit shift builtin ops ([#145802](https://github.com/pytorch/pytorch/pull/145802))
- bump counters for unbacked binding names ([#145882](https://github.com/pytorch/pytorch/pull/145882))
- [export] Add tlparse to draft-export ([#145810](https://github.com/pytorch/pytorch/pull/145810))
- Introduce aoti_call_delegate HOP ([#145630](https://github.com/pytorch/pytorch/pull/145630))
- [export] nested terms in nn_module_stack deserialization ([#145901](https://github.com/pytorch/pytorch/pull/145901))
- [export] Sync model container types to schema.py ([#145959](https://github.com/pytorch/pytorch/pull/145959))
- fix internal error with reorder submodules ([#146181](https://github.com/pytorch/pytorch/pull/146181))
- [export] Fix symfloat serialization ([#146112](https://github.com/pytorch/pytorch/pull/146112))
- [export][ez] Fix generated header file. ([#146208](https://github.com/pytorch/pytorch/pull/146208))
- Add buffers to parameterizaiton rule ([#145991](https://github.com/pytorch/pytorch/pull/145991))
- [export] Additionally save pytree namedtuple field names ([#145956](https://github.com/pytorch/pytorch/pull/145956))
- [export] Fix requires_grad deserialization ([#146351](https://github.com/pytorch/pytorch/pull/146351))
- [export] Include metadata in FlatArgsAdapter ([#146107](https://github.com/pytorch/pytorch/pull/146107))
- [export] Fix draft-export logging ([#146106](https://github.com/pytorch/pytorch/pull/146106))
- [export] make stack_trace optional in insert_custom_op_guards ([#146438](https://github.com/pytorch/pytorch/pull/146438))
- [sigmoid] Implement a OSS only model runner. ([#146440](https://github.com/pytorch/pytorch/pull/146440))
- [export][ez] Allow math.trunc for serialization. ([#146715](https://github.com/pytorch/pytorch/pull/146715))
- Implement serializable getattr support for tensor subclasses ([#145772](https://github.com/pytorch/pytorch/pull/145772))
- [export] Serialize special values of float into strings for json. ([#146490](https://github.com/pytorch/pytorch/pull/146490))
- [export][ez] Update tag_ for union setters. ([#146912](https://github.com/pytorch/pytorch/pull/146912))
- [export] Dedup expression_created logs ([#146859](https://github.com/pytorch/pytorch/pull/146859))
- [export] Generate printers/parsers for serialization enum values. ([#147126](https://github.com/pytorch/pytorch/pull/147126))
- [export] Loosen symint input serialization ([#147237](https://github.com/pytorch/pytorch/pull/147237))
- [apf] Fix input adapter ([#147238](https://github.com/pytorch/pytorch/pull/147238))
- Re-land exclude upsample_bilinear2d.vec and nearest2d.vec from default export decomposition table ([#147153](https://github.com/pytorch/pytorch/pull/147153))
- [draft_export] only clear pending unbacked symbols for overwritten kernels ([#147427](https://github.com/pytorch/pytorch/pull/147427))
- [torchbind] Differentiate ScriptModule and ScriptObject with qualified name ([#147399](https://github.com/pytorch/pytorch/pull/147399))
- reland "[sigmoid] Test OSS model runner with test_export.py" ([#147535](https://github.com/pytorch/pytorch/pull/147535))
- better error message ([#147532](https://github.com/pytorch/pytorch/pull/147532))
- specify only some dimensions in shapes collection ([#147534](https://github.com/pytorch/pytorch/pull/147534))
- [export] Remove report from draft-export output ([#147558](https://github.com/pytorch/pytorch/pull/147558))
- export method ([#147573](https://github.com/pytorch/pytorch/pull/147573))
- [export] Sync aoti schema to schema.py ([#148017](https://github.com/pytorch/pytorch/pull/148017))
- [dynamic shapes][export] ignore when real-tensor fallback fails ([#147779](https://github.com/pytorch/pytorch/pull/147779))
- fix lost input mutations with export_tracepoint ([#148709](https://github.com/pytorch/pytorch/pull/148709))
- Fix "invalid application of 'sizeof' to an incomplete type" ([#148854](https://github.com/pytorch/pytorch/pull/148854))
### not user facing
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
