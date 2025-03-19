
# Release Notes worksheet fx

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

## fx
### bc breaking
### deprecation
### new features
### improvements
### bug fixes
### performance
### docs
### devs
### Untopiced
- No actual change, just remove variable contain Tensors from global scope ([#143225](https://github.com/pytorch/pytorch/pull/143225))
- Move inner loop of _create_symbolic_sizes_strides_storage_offset into its own method ([#138843](https://github.com/pytorch/pytorch/pull/138843))
- Simplify _compute_symbolic_stride() ([#138844](https://github.com/pytorch/pytorch/pull/138844))
- subgraph rewriter supports matched pattern with no users ([#143842](https://github.com/pytorch/pytorch/pull/143842))
- Fix get_source_partitions when weights are tied ([#142446](https://github.com/pytorch/pytorch/pull/142446))
- [TGIF][Easy] Slightly improve the logging for tgif split pass ([#143771](https://github.com/pytorch/pytorch/pull/143771))
- [aot] don't dce aten rng nodes ([#144319](https://github.com/pytorch/pytorch/pull/144319))
- Micro-optimization in Graph.nodes.__iter__ ([#144631](https://github.com/pytorch/pytorch/pull/144631))
- [BE]: Update literal typing for torch/fx/graph nodelist ([#144650](https://github.com/pytorch/pytorch/pull/144650))
- Add max kwarg to torch._check with alternate size oblivious semantics ([#144471](https://github.com/pytorch/pytorch/pull/144471))
- [BE] Remove lambda from str ([#144743](https://github.com/pytorch/pytorch/pull/144743))
- [export] Support module inputs for non strict mode. ([#143925](https://github.com/pytorch/pytorch/pull/143925))
- [BE]: Improve typing for torch/fx/_pytree.py and torch/utils/_pytree.py ([#145173](https://github.com/pytorch/pytorch/pull/145173))
- Don't overspecialize float when propagating cache guards to ShapeEnv ([#145078](https://github.com/pytorch/pytorch/pull/145078))
- Unconditionally exclude upper bound in all size oblivious tests ([#144867](https://github.com/pytorch/pytorch/pull/144867))
- [fx] move DCE rand check to import time ([#145118](https://github.com/pytorch/pytorch/pull/145118))
- Fix for failure in D68425364 ([#145304](https://github.com/pytorch/pytorch/pull/145304))
- Fix incorrect type comparison ([#145449](https://github.com/pytorch/pytorch/pull/145449))
- setitem node shouldn't be deadcode eliminated ([#145714](https://github.com/pytorch/pytorch/pull/145714))
- [export] fix non-strict pre_dispatch exporting while_loop ([#145762](https://github.com/pytorch/pytorch/pull/145762))
- ehnace logging statically known by adding size_oblivious(..) ([#145354](https://github.com/pytorch/pytorch/pull/145354))
- include entire GraphModule instead of current node when erroring inside of fx interpreter ([#146197](https://github.com/pytorch/pytorch/pull/146197))
- Barebones flat_apply HOP ([#146060](https://github.com/pytorch/pytorch/pull/146060))
- Fix constants with non-functional operators ([#145593](https://github.com/pytorch/pytorch/pull/145593))
- [dynamic shapes][real tensor tracing] propagate unbacked hint when creating mod replacement ([#146381](https://github.com/pytorch/pytorch/pull/146381))
- [export][dynamic shapes] log provenance for locals & symbols for non-strict ([#143378](https://github.com/pytorch/pytorch/pull/143378))
- Move capture_provenance to make_node_impl ([#146625](https://github.com/pytorch/pytorch/pull/146625))
- [symbolic shapes] Log symnode id ([#146583](https://github.com/pytorch/pytorch/pull/146583))
- Fix lint ([#146846](https://github.com/pytorch/pytorch/pull/146846))
- [oncall] Change error message to be more readable ([#146934](https://github.com/pytorch/pytorch/pull/146934))
- [symbolic shapes] Log SymNode id for provenance ([#146532](https://github.com/pytorch/pytorch/pull/146532))
- [export] Use custom stream logger in draft-export ([#146533](https://github.com/pytorch/pytorch/pull/146533))
- [export] Add additional tlparse logging ([#146534](https://github.com/pytorch/pytorch/pull/146534))
- [tlparse] Add stacktrace filter utility ([#146858](https://github.com/pytorch/pytorch/pull/146858))
- [export] Minor fix to locals ([#146955](https://github.com/pytorch/pytorch/pull/146955))
- [export] Log evaluate_expr ([#146939](https://github.com/pytorch/pytorch/pull/146939))
- [symbolic shapes] Add replacement for backed symints ([#147240](https://github.com/pytorch/pytorch/pull/147240))
- [fx] demote node prepend to self log from warning to debug ([#147538](https://github.com/pytorch/pytorch/pull/147538))
- [FX] Refactor immutable collections implementation ([#144640](https://github.com/pytorch/pytorch/pull/144640))
- [Minor] Fix minor mistake in docstring of replace_pattern ([#147611](https://github.com/pytorch/pytorch/pull/147611))
- Fix register constant to be usable in exportz ([#147533](https://github.com/pytorch/pytorch/pull/147533))
- only print GraphModule during fx.Interpreter errors if valid ([#148090](https://github.com/pytorch/pytorch/pull/148090))
- [export] Fix logging so that it doesn't result in max recursion error ([#148231](https://github.com/pytorch/pytorch/pull/148231))
- [export] don't use unbacked_renamings in export ([#147574](https://github.com/pytorch/pytorch/pull/147574))
- [export] Fix AttrProxy slicing ([#148507](https://github.com/pytorch/pytorch/pull/148507))
- [Minimizer] allow overriding of ShapeProp logic by subclasses of _MinimizerBase ([#148784](https://github.com/pytorch/pytorch/pull/148784))
- partitioner: when materializing unbacked tensor intermediates, apply hint to symbol, not expr ([#144097](https://github.com/pytorch/pytorch/pull/144097))
- [dynamic shapes] add backed_size_oblivious option ([#148696](https://github.com/pytorch/pytorch/pull/148696))
### not user facing
- Add <string> to uninteresting_files ([#142984](https://github.com/pytorch/pytorch/pull/142984))
- Add torch._compile to uninteresting files ([#143209](https://github.com/pytorch/pytorch/pull/143209))
- remove allow-untyped-defs for torch/fx/experimental/debug.py ([#143439](https://github.com/pytorch/pytorch/pull/143439))
- Don't 1 specialize if stride is contiguous ([#143365](https://github.com/pytorch/pytorch/pull/143365))
- Unbacked SymInt fixes for subclasses + data-dependent slice() bounds (non-dynamic) ([#143526](https://github.com/pytorch/pytorch/pull/143526))
- add some logging for tensorify ([#143391](https://github.com/pytorch/pytorch/pull/143391))
- Unbacked SymInt fixes for subclasses + data-dependent slice() bounds ([#142062](https://github.com/pytorch/pytorch/pull/142062))
- remove allow-untyped-defs from torch/fx/experimental/refinement_types.py ([#143602](https://github.com/pytorch/pytorch/pull/143602))
- Revert "refactor tensorify restart logic to use sources (#141517)" ([#143623](https://github.com/pytorch/pytorch/pull/143623))
- remove allow-untyped-defs from fx/experimental/refinement_types.py ([#143868](https://github.com/pytorch/pytorch/pull/143868))
- detect fake mode in proxy_tensor creation in make_fx ([#144168](https://github.com/pytorch/pytorch/pull/144168))
- Fix deepcopy hooks ([#144531](https://github.com/pytorch/pytorch/pull/144531))
- [BE] Make a SymbolInfo NamedTuple ([#144745](https://github.com/pytorch/pytorch/pull/144745))
- Downgrade ignored guard to info level ([#145075](https://github.com/pytorch/pytorch/pull/145075))
- PEP585 update - torch/fx ([#145166](https://github.com/pytorch/pytorch/pull/145166))
- [BE] Fix edge case in translation validation bisector ([#145414](https://github.com/pytorch/pytorch/pull/145414))
- Allow replacing unbacked with very large upperbound by returning no-op for FloorToInt(int) ([#146001](https://github.com/pytorch/pytorch/pull/146001))
- add node mapping processing ([#146103](https://github.com/pytorch/pytorch/pull/146103))
- log out partial fx graph when guard on data dependent during non stirct tracing ([#146298](https://github.com/pytorch/pytorch/pull/146298))
- Integrate sympy expression provenance logging with structured logs ([#145848](https://github.com/pytorch/pytorch/pull/145848))
- add support for capturing provenance of unary operations ([#146413](https://github.com/pytorch/pytorch/pull/146413))
- add support for capturing provenance of unary operations ([#146413](https://github.com/pytorch/pytorch/pull/146413))
- Add self to CODEOWNERS for fx/proxy.py; warn against adding new node arg types ([#147031](https://github.com/pytorch/pytorch/pull/147031))
- Make fx.node.map_arg() and .map_aggregate() generic ([#146248](https://github.com/pytorch/pytorch/pull/146248))
- Add no_data_dependent_graph_break mode ([#147342](https://github.com/pytorch/pytorch/pull/147342))
- add unbacked strict mode ([#147333](https://github.com/pytorch/pytorch/pull/147333))
- Support size oblivious max equation ([#147344](https://github.com/pytorch/pytorch/pull/147344))
- [FX] micro-optimization `map_aggregate(immutable_dict)` ([#147691](https://github.com/pytorch/pytorch/pull/147691))
- [inductor] Improve type annotations in _inductor/pattern_matcher.py ([#146626](https://github.com/pytorch/pytorch/pull/146626))
- introduce dynamism library ([#147981](https://github.com/pytorch/pytorch/pull/147981))
- introduce dynamism library ([#147981](https://github.com/pytorch/pytorch/pull/147981))
- introduce dynamism library ([#147981](https://github.com/pytorch/pytorch/pull/147981))
- Fix recent regression in evaluate_expr that effect cache lookups ([#147836](https://github.com/pytorch/pytorch/pull/147836))
### security
