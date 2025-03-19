
# Release Notes worksheet optim

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

## optim
### bc breaking
**`LRScheduler.print_lr()` along with the `verbose` kwarg to the LRScheduler constructor has been deprecated since release 2.2. Please use `LRScheduler.get_last_lr()` to access the learning rate instead.** (#147301)

`print_lr` and `verbose` were confusing, not properly documented and were little used, as described in #99270, so we deprecated them in 2.2. Now, we complete the deprecation by removing them completely. To access and print the learning rate of a LRScheduler:

In 2.6.0
```
optim = ...
lrsched = torch.optim.lr_scheduler.ReduceLROnPlateau(optim, verbose=True)
// lrsched will internally call print_lr
```

In 2.7.0
```
optim = ...
lrsched = torch.optim.lr_scheduler.ReduceLROnPlateau(optim)
print(lrsched.get_last_lr())
```

### deprecation
### new features
### improvements
- Refactor AdamW to subclass Adam (#143710, #144972)
- Add support for differentiable LR and weight_decay in SGD, Adam(W) (#143510, #143679, #143726)
### bug fixes
### performance
### docs
- Clarify what we mean by decoupled weight decay in the *AdamWs (#144101, #144984)
- Corrected description of AMSGrad algorithm (#142351)

### devs
### Untopiced
### not user facing
- PEP585 update -  torch/nn torch/optim torch/package torch/profiler torch/serialization torch/sparse torch/xpu ([#145175](https://github.com/pytorch/pytorch/pull/145175))
- PEP585 update -  torch/nn torch/optim torch/package torch/profiler torch/serialization torch/sparse torch/xpu ([#145175](https://github.com/pytorch/pytorch/pull/145175))
- Add LBFGS params optional desc ([#147579](https://github.com/pytorch/pytorch/pull/147579))
- Removed unused _RequiredParameter ([#144771](https://github.com/pytorch/pytorch/pull/144771)) (was reverted)
### security
