
# Release Notes worksheet dynamo

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

## dynamo
### bc breaking
### deprecation
- Dropped support for Python < 3.9 (#147097)
- Compiled Autograd dropped annotation requirements for custom autograd functions (#146229, #146720)
### new features
- Support tracing `contextlib.contextmanager` in Dynamo (#136033)
- `nonstrict_trace` escape hatch to apply non-strict tracing to difficult-to-compile code (#146367)
- Delayed compile for dynamic shapes (#147983)
- Support tracing generators (#141055)
- Whitelist of source files to apply dynamic shapes to (#147979)
- Support tracing `list` subclasses (#146819)


### improvements
- Better tracing support for user-defined `dict` subclasses (#143548)
- Improved graph break messages for some common graph break sites (#146525)
- Improved tracing of exceptions (#146492)
- Remove a number of builtin and third-party modules from `trace_rules.py` skipfiles (#145856)
- Remove some specialized variables for specific third-party classes (e.g. `transformers` `ModelOutput`) (#143567)


### bug fixes
- Guard on global autocast state (#143592)
- Fix some internal crashes involving undefined names (#144784)
- Multiple silent incorrectness fixes for Compiled Autograd (#144707)
- Fix graph break in FlexAttention when using Compiled Autograd (#144533)

### performance
- Implement dynamic shape guards in C++ (#139899)
- Directly access Python frame locals in guard checks (#140063)
- Misc. Dynamo tracing time improvements (#143066)


### docs
- Remove the suggestion to use `suppress_errors` on compiler error (#146553)
- Automatically generated Dynamo docs (#146736)

### devs
- New internal graph break API that enforces better error messages (#146525)
- Replace internal calls to `torch._dynamo.optimize()` with `torch.compile()` (#142451)


### Untopiced
### not user facing
### security
