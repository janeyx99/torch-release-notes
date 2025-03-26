
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
- Fix subgraph rewriter to support matched pattern with no users (#143842)
- Improve error message to include entire GraphModule (#146197, #148090)
- Allow overriding of ShapeProp (#148784)
### bug fixes
- Fix `get_source_partitions` when weights are tied (#142446)
- Prevent DCE of ATen rng nodes (#144319)
- Fix incorrect type comparison (#145449)
- Fix DCE of setitem node (#145714)
- Fix pytree.register_constant to be usable in export (#147533)
- Fix edge case in translation validation bisector (#145414)
### performance
- Micro-optimization in `Graph.nodes.__iter__` (#144631)
- Micro-optimization in `map_aggregate(immutable_dict)` (#147691)
- Move DCE rand check to import time (#145118)
### docs
- Improve logging for splitter (#143771)
- Update literal typing for torch/fx/graph nodelist (#144650)
- Improve typing for torch/fx/_pytree.py and torch/utils/_pytree.py (#145173)
- Fix minor mistake in docstring of replace_pattern (#147611)
### devs
- Downgrade some logs (#147538, #145075)
- Refactor immutable collections implementation (#144640)
- Make `fx.node.map_arg()` and `.map_aggregate()` generic (#146248)
### Untopiced




### not user facing
- Use custom stream logger in draft-export (#146533, #146534, #148231)
- Avoid using unbacked_renamings in export (#147574)
- Fix AttrProxy slicing (#148507)
- Add files to uninteresting_files (#142984, #143209)
- Remove allow-untyped-defs for files (#143439, #143602, #143868)
- Don't 1 specialize if stride is contiguous (#143365)
- Unbacked SymInt fixes for subclasses + data-dependent slice() bounds (#143526, #142062)
- Add logging for tensorify (#143391)
- Detect fake mode in proxy_tensor creation in make_fx (#144168)
- Fix deepcopy hooks (#144531)
- Make a SymbolInfo NamedTuple (#144745)
- PEP585 update of torch/fx (#145166)
- Allow replacing unbacked with very large upperbound by returning no-op for FloorToInt(int) (#146001)
- Add node mapping processing (#146103)
- Support size oblivious max equation (#147344)
- Improve type annotations in _inductor/pattern_matcher.py (#146626)
- Add self to CODEOWNERS for fx/proxy.py; warn against adding new node arg types (#147031)

### security
