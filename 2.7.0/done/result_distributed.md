
# Release Notes worksheet distributed

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

## distributed
### bc breaking
### deprecation
### new features
- Context parallel
  - We provided a Context Parallel API ([#131351](https://github.com/pytorch/pytorch/pull/131351)) for users to parallelize `torch.nn.functional.scaled_dot_product_attention` over the sequence dimension. We implemented
  Ring Attention ([#131351](https://github.com/pytorch/pytorch/pull/131351)) and an AllGather-based approach ([#132820](https://github.com/pytorch/pytorch/pull/132820)) where the all-gather is issued before the first local SDPA
  and the subsequent local SDPAs will have to wait until the all-gather completes, and offered a user API ([#142093](https://github.com/pytorch/pytorch/pull/142093)) to select the desired approach. The implementation
  currently supports three SDPA kernels: `SDPBackend.FLASH_ATTENTION`, `SDPBackend.EFFICIENT_ATTENTION`, and `SDPBackend.CUDNN_ATTENTION` ([#148537](https://github.com/pytorch/pytorch/pull/148537)). We also
  verified that our Context Parallel implementation is compatible with other parallelisms and `torch.compile`.
- c10d
  - Implemented ncclCommInitRankScalable (merging #136789) ([#144794](https://github.com/pytorch/pytorch/pull/144794))
### improvements
- DistributedDataParallel (DDP)
  - Added `init_sync` option to control collectives during initialization ([#142824](https://github.com/pytorch/pytorch/pull/142824))
  - Decoupled python reducer from compilation mode ([#147123](https://github.com/pytorch/pytorch/pull/147123))
- c10d
  - Simplified `abort` and `shutdown` by adding both to `Backend` and `ProcessGroup` objects ([#148798](https://github.com/pytorch/pytorch/pull/148798))
  - Used `new_group` instead of `split_group` on non-CUDA device ([#141469](https://github.com/pytorch/pytorch/pull/141469))
  - Removed `call_guard` in pybind object init of c10d ([#143598](https://github.com/pytorch/pytorch/pull/143598))
  - Enabled coalescing path on XPU and dispatch to XPU tensor barrier if XCCL backend is specified. ([#143735](https://github.com/pytorch/pytorch/pull/143735))
  - Preserved PyWork's Python reference counting when used in functional collectives ([#146376](https://github.com/pytorch/pytorch/pull/146376))
  - Enabled soft fail bind when agent store active inside TCPStore ([#147465](https://github.com/pytorch/pytorch/pull/147465))
  - Made `getDefaultBackend` more fault tolerant ([#148596](https://github.com/pytorch/pytorch/pull/148596))
- DTensor
  - Added `aten.amin/amax` to `linear_reduction_strategy` ([#143747](https://github.com/pytorch/pytorch/pull/143747))
  - Added `src_data_rank` to `distribute_tensor` API ([#143883](https://github.com/pytorch/pytorch/pull/143883))
  - Added strategy for `_scaled_mm` ([#143760](https://github.com/pytorch/pytorch/pull/143760))
  - Added `aten.view.dtype` op support ([#144404](https://github.com/pytorch/pytorch/pull/144404))
  - Enabled sharding prop to handle cross mesh computation ([#147869](https://github.com/pytorch/pytorch/pull/147869))
  - Added CuDNN SDPA op support to DTensor ([#148537](https://github.com/pytorch/pytorch/pull/148537))
  - Optimized `shard_dim_alltoall` to use `alltoall_single` ([#148868](https://github.com/pytorch/pytorch/pull/148868))
  - Deprecated `_shard_tensor` to use `src_data_rank=None` ([#144171](https://github.com/pytorch/pytorch/pull/144171))
  - Added pointwise ops strategy for `aten.minimum` ([#145816](https://github.com/pytorch/pytorch/pull/145816))

- FullyShardedDataParallel2 (FSDP2)
  - Clamp `reduce_dtype` in lazy init ([#143297](https://github.com/pytorch/pytorch/pull/143297))
  - Enabled FSDP2 on XPU device ([#143737](https://github.com/pytorch/pytorch/pull/143737))
  - Made post-backward condition more robust ([#144781](https://github.com/pytorch/pytorch/pull/144781))
  - Enabled MTIA device in FSDP2 library code ([#145842](https://github.com/pytorch/pytorch/pull/145842))
  - Avoided resetting version counter of all_gather_output in inference_mode ([#146709](https://github.com/pytorch/pytorch/pull/146709))
  - Supported ignoring parameters in FSDP2 ([#146631](https://github.com/pytorch/pytorch/pull/146631))
  - Enabled FSDP tests on XPU device ([#147518](https://github.com/pytorch/pytorch/pull/147518))
  - Enabled FSDP2 on HPU device ([#148667](https://github.com/pytorch/pytorch/pull/148667))
- TensorParallel
  - Propagated `src_data_rank` kwarg in TP API ([#144005](https://github.com/pytorch/pytorch/pull/144005))
- torchelastic
  - Added kill logic for current process when killing a worker ([#141060](https://github.com/pytorch/pytorch/pull/141060))
  - Made `etcd_rendezvous` publicly importable ([#145396](https://github.com/pytorch/pytorch/pull/145396))
  - Exposed the rendezvous keepalive arguments ([#145228](https://github.com/pytorch/pytorch/pull/145228))
- Pipelining
  - Added `generate_stage_to_rank_mapping` utility ([#146193](https://github.com/pytorch/pytorch/pull/146193))
  - Removed `stage_index_to_group_rank` from schedule ([#146217](https://github.com/pytorch/pytorch/pull/146217))
### bug fixes
- c10d
  - Fixed `CudaEventCache` for dangling references ([#144496](https://github.com/pytorch/pytorch/pull/144496))
  - Make `all-reduce` input contiguous in `distributed.nn.all_reduce` ([#144267](https://github.com/pytorch/pytorch/pull/144267))
  - Removed `Alltoallv` specialization for PyTorch generic `all_to_all` ([#145045](https://github.com/pytorch/pytorch/pull/145045))
  - Added a handle case when remote peer closes connection for TCPStore ([#145757](https://github.com/pytorch/pytorch/pull/145757))
  - Fixed memory leak on shutdown ([#145507](https://github.com/pytorch/pytorch/pull/145507))
  - Fixed an issue where functional collectives don't force fx stride on inputs when compiled ([#146467](https://github.com/pytorch/pytorch/pull/146467))
  - Associated tensor allocation support with NCCL version ([#146842](https://github.com/pytorch/pytorch/pull/146842))
  - Modified API to get device string from device with `torch.device` ([#146290](https://github.com/pytorch/pytorch/pull/146290))
  - Fixed `dist.init_process_group` on windows ([#148266](https://github.com/pytorch/pytorch/pull/148266))
  - Fixed capturability of `isend` and `irecv` ([#148462](https://github.com/pytorch/pytorch/pull/148462))
- DistributedStateDict (DSD)
  - Fixed `strict=False` case for DDP ([#143038](https://github.com/pytorch/pytorch/pull/143038))
  - Fixed issue when there is a PG without parameters ([#147730](https://github.com/pytorch/pytorch/pull/147730))
  - Fixed the shared parameter mismatch for optimizer state_dict when flattening FQNs are used ([#148825](https://github.com/pytorch/pytorch/pull/148825))
- DTensor
  - Fixed `torch.distributed._functional_collectives.AsyncCollectiveTensor` for `aten.to`. ([#134661](https://github.com/pytorch/pytorch/pull/134661))
  - Deferred DTensor RNG state sync until first random op call or manual_seed call to support more flexible OffsetBasedRNGTracker init ([#147025](https://github.com/pytorch/pytorch/pull/147025))
  - Fixed `_scaled_dot_product_flash_attention` sharding ([#148125](https://github.com/pytorch/pytorch/pull/148125))
  - Fixed redistribution cost for `all-reduce` ([#148761](https://github.com/pytorch/pytorch/pull/148761))

- FullyShardedDataParallel2 (FSDP2)
  - Rooted fix for FP8 tensor ([#143248](https://github.com/pytorch/pytorch/pull/143248))
  - Added workaround to fix `buffer_dtype` without root parameters ([#143989](https://github.com/pytorch/pytorch/pull/143989))
  - Supported custom all reduce hook across FSDP units ([#147114](https://github.com/pytorch/pytorch/pull/147114))
  - Fixed bug in FSDP wrapped module with zero argument  ([#147771](https://github.com/pytorch/pytorch/pull/147771))
- pipelining
  - Fixed backward_one_chunk when the output of the model is a view ([#142237](https://github.com/pytorch/pytorch/pull/142237))
  - Threw error with ZB and compile ([#143599](https://github.com/pytorch/pytorch/pull/143599))
  - Fixed FSDP+PP stream sync bug ([#144535](https://github.com/pytorch/pytorch/pull/144535))
  - Fixed PP grad scaling ([#144352](https://github.com/pytorch/pytorch/pull/144352))
  - No allowing for num_microbatches > num_stages for single stage schedules ([#144702](https://github.com/pytorch/pytorch/pull/144702))
  - Fixed shape_inference for V-schedules ([#147000](https://github.com/pytorch/pytorch/pull/147000))
### performance
- c10d
  - Changed `ALLOC_BUFFER_SIZE` from 4000 to 4096 to be a power of 2 for TCPStore ([#145759](https://github.com/pytorch/pytorch/pull/145759))
  - Improved IPC tensor release performance by releasing the IpcMutex when deleting the `ExpandableSegements` object and the GIL in WorkNCCL destructor ([#148805](https://github.com/pytorch/pytorch/pull/148805))
### docs
- c10d
  - Updated docs for `wait()` ([#143305](https://github.com/pytorch/pytorch/pull/143305))
  - Added comments to the end of Macro for better readability ([#144789](https://github.com/pytorch/pytorch/pull/144789))
- DeviceMesh
  - Added some documentation for `from_group` API and add a 2D test ([#146364](https://github.com/pytorch/pytorch/pull/146364))
- DTensor
  - Expose the `__create_chunk_list__` in the doc ([#144100](https://github.com/pytorch/pytorch/pull/144100))
- DistributedStateDict (DSD)
  - Updated the document to mention the limitation of `set_optimizer_state_dict` ([#148918](https://github.com/pytorch/pytorch/pull/148918))
- FullyShardedDataParallel2 (FSDP2)
  - Highlighted equivalence of `set_requires_gradient_sync` and `no_sync` ([#148715](https://github.com/pytorch/pytorch/pull/148715))
- torchelastic
  - Replaced incorrect .. note:: invocations ([#142868](https://github.com/pytorch/pytorch/pull/142868))
  - Fixed the doc string for `record` ([#146968](https://github.com/pytorch/pytorch/pull/146968))
- pipelining
  - Updated tutorials and documentation ([#143045](https://github.com/pytorch/pytorch/pull/143045))
### devs
- c10d
  - Improved the dump mechanism for flight recorder ([#143446](https://github.com/pytorch/pytorch/pull/143446))
  - Added log trace capture enabled or not in flight recorder ([#143865](https://github.com/pytorch/pytorch/pull/143865))
  - Added file flush in file based dumper of flight recorder ([#145458](https://github.com/pytorch/pytorch/pull/145458))
  - Caught c10 error and log message inside monitoring thread ([#145413](https://github.com/pytorch/pytorch/pull/145413))
  - Added an API to get the status/error code at the PG level ([#144498](https://github.com/pytorch/pytorch/pull/144498))
  - Moved record param for init to the right place ([#148571](https://github.com/pytorch/pytorch/pull/148571))
  - Enabled testing generelization for multiple accelerator devices ([#139749](https://github.com/pytorch/pytorch/pull/139749))

- FullyShardedDataParallel2 (FSDP2)
  - Enabled the typing of `fully_shard` so that the return value can be chained with typing enabled ([#147489](https://github.com/pytorch/pytorch/pull/147489))
- TensorParallel
  - Added warning when module is distributed twice ([#147006](https://github.com/pytorch/pytorch/pull/147006))
- Pipelining
  - Improved shape inference debug logging ([#144929](https://github.com/pytorch/pytorch/pull/144929))


### Untopiced
### not user facing
- [PP] Remove extra code and docs BE ([#147636](https://github.com/pytorch/pytorch/pull/147636))
- [BE] TCPStore: use typed errors for assertions ([#147647](https://github.com/pytorch/pytorch/pull/147647))
- capture the return value in the contract typing ([#147488](https://github.com/pytorch/pytorch/pull/147488))
- Code Refactoring for getting start and stride from global ranks ([#147230](https://github.com/pytorch/pytorch/pull/147230))
- Remove outdated CUDA version check ([#148142](https://github.com/pytorch/pytorch/pull/148142))
- chore: fix typos in error messages in FSDP ([#146805](https://github.com/pytorch/pytorch/pull/146805))
- [chore] fix new linter ([#145756](https://github.com/pytorch/pytorch/pull/145756))
- [c10d][ez] Remove goto in PGNCCL and make linter happy for PGNCCL and NCCLUtils ([#145855](https://github.com/pytorch/pytorch/pull/145855))
- [2/N] Enable ruff F841 on distributed tests ([#146132](https://github.com/pytorch/pytorch/pull/146132))
- Remove some NOLINT ([#146610](https://github.com/pytorch/pytorch/pull/146610))
- Cleanup CallOnce.h ([#146700](https://github.com/pytorch/pytorch/pull/146700))
- revert PTD's change that leads to signature mismatch of printNcclCommProxyTrace ([#146453](https://github.com/pytorch/pytorch/pull/146453))
- [4/N] Remove unnecessary once flag usage ([#146783](https://github.com/pytorch/pytorch/pull/146783))
- Remove unnecessary once flag usage ([#143255](https://github.com/pytorch/pytorch/pull/143255))
- Drop unused num_elements variable ([#144723](https://github.com/pytorch/pytorch/pull/144723))
- Avoid running helper functions as test ([#144544](https://github.com/pytorch/pytorch/pull/144544))
- [PTD] Dump rcclexp proxy trace in pytorch ([#143678](https://github.com/pytorch/pytorch/pull/143678))
- Enable more C++ warnings ([#143099](https://github.com/pytorch/pytorch/pull/143099))
- [PGNCCL] Move NCCLComm impl to cpp ([#142826](https://github.com/pytorch/pytorch/pull/142826))
- [dtensor] fix side-effect on dtype for _like ops ([#146869](https://github.com/pytorch/pytorch/pull/146869))
- [fused_all_gather_matmul] introduce an argument to specify whether the all-gather result needs to be returned ([#143159](https://github.com/pytorch/pytorch/pull/143159))
- remove allow-untyped-defs from torch/distributed/elastic/multiprocessing/errors/handlers.py ([#143605](https://github.com/pytorch/pytorch/pull/143605))
- remove allow-untyped-defs from distributed/elastic/multiprocessing/errors/handlers.py ([#143869](https://github.com/pytorch/pytorch/pull/143869))
- remove allow-untyped-defs from distributed/elastic/multiprocessing/subprocess_handler/handlers.py ([#143917](https://github.com/pytorch/pytorch/pull/143917))
- remove allow-untyped-defs from distributed/elastic/utils/data/cycling_iterator.py ([#144090](https://github.com/pytorch/pytorch/pull/144090))
- PEP585 update - torch/distributed/elastic torch/distributed/checkpoint ([#145163](https://github.com/pytorch/pytorch/pull/145163))
- Remove NO_MULTIPROCESSING_SPAWN checks ([#146705](https://github.com/pytorch/pytorch/pull/146705))
- Remove NO_MULTIPROCESSING_SPAWN checks ([#146705](https://github.com/pytorch/pytorch/pull/146705))
- Re-enable some C++ warnings ([#142332](https://github.com/pytorch/pytorch/pull/142332))
- [DTensor][random] add HSDP+TP model init test ([#143077](https://github.com/pytorch/pytorch/pull/143077))
- Enable cutlass-based all-gather matmul when TORCH_SYMM_MEM_ENABLE_NATIVE_ASYNC_TP is set ([#142283](https://github.com/pytorch/pytorch/pull/142283))
- [distributed] Remove unused variable in test_composability/test_pp_composability.py ([#143191](https://github.com/pytorch/pytorch/pull/143191))
- remove allow-untyped-defs for distributed/rpc/_testing/__init__.py ([#143271](https://github.com/pytorch/pytorch/pull/143271))
- [BE] typing for decorators - distributed/_tensor/ops/utils ([#142139](https://github.com/pytorch/pytorch/pull/142139))
- [fused_all_gather_matmul] use _multimem_all_gather_matmul for small global Ms ([#143160](https://github.com/pytorch/pytorch/pull/143160))
- remove allow-untyped-defs for torch/_C/_distributed_autograd.pyi ([#143369](https://github.com/pytorch/pytorch/pull/143369))
- [fr] recognize all_reduce_barrier as a valid op ([#143354](https://github.com/pytorch/pytorch/pull/143354))
- [state dict] Change _load_model_state_dict to enable cpu_offload, accept 2 device type and optimize memory ([#142845](https://github.com/pytorch/pytorch/pull/142845))
- remove allow-untyped-defs from distributed/tensor/experimental/__init__.py ([#143583](https://github.com/pytorch/pytorch/pull/143583))
- remove allow-untyped-defs from torch/distributed/pipelining/_debug.py ([#143606](https://github.com/pytorch/pytorch/pull/143606))
- [micro_pipeline_tp] don't pass return_A to fused_all_gather_scaled_matmul ([#143782](https://github.com/pytorch/pytorch/pull/143782))
- [BE][CI] bump `ruff` to 0.8.4 ([#143753](https://github.com/pytorch/pytorch/pull/143753))
- remove allow-untyped-defs from torch/distributed/pipelining/_debug.py ([#143871](https://github.com/pytorch/pytorch/pull/143871))
- [fr][c10d] fix flaky test ([#143878](https://github.com/pytorch/pytorch/pull/143878))
- remove allow-untyped-defs from distributed/pipelining/_unflatten.py ([#143915](https://github.com/pytorch/pytorch/pull/143915))
- remove allow-untyped-defs from torch/distributed/fsdp/_dynamo_utils.py ([#144131](https://github.com/pytorch/pytorch/pull/144131))
- [ROCm] CK Flash Attention Backend ([#143695](https://github.com/pytorch/pytorch/pull/143695))
- Fix incorrect python expression ([#143675](https://github.com/pytorch/pytorch/pull/143675))
- [BE]: Remove redundant copy in torch chunk shard ([#144269](https://github.com/pytorch/pytorch/pull/144269))
- [AsyncMM] re-enable and prepare for cutlass 3.5.1 update ([#144011](https://github.com/pytorch/pytorch/pull/144011))
- [Pipelining] Refactor pp composability test to use faster MPCT ([#144345](https://github.com/pytorch/pytorch/pull/144345))
- Migrate from Tuple -> tuple in torch/testing ([#144256](https://github.com/pytorch/pytorch/pull/144256))
- Migrate from Tuple -> tuple in test/distributed/_composable ([#144254](https://github.com/pytorch/pytorch/pull/144254))
- Migrate from Tuple -> tuple in torch/distributed ([#144258](https://github.com/pytorch/pytorch/pull/144258))
- [distributed] Fix _ReaderView.read() and readinto() to stop reading at the end of the slice ([#143357](https://github.com/pytorch/pytorch/pull/143357))
- [Pipelining] Improve test_pp_dp ([#144534](https://github.com/pytorch/pytorch/pull/144534))
- remove allow-untyped-defs from torch/distributed/_checkpointable.py ([#144627](https://github.com/pytorch/pytorch/pull/144627))
- remove allow-untyped-defs from torch/distributed/_shard/sharded_tensor/shard.py ([#144623](https://github.com/pytorch/pytorch/pull/144623))
- remove allow-untyped-defs from torch/distributed/checkpoint/api.py ([#144653](https://github.com/pytorch/pytorch/pull/144653))
- ROCm: Skip tests in elastic/utils/distributed_test ([#144692](https://github.com/pytorch/pytorch/pull/144692))
- [Pipelining] Refactor common utils from test_pp_dp ([#144596](https://github.com/pytorch/pytorch/pull/144596))
- [Pipelining] fix test_schedule.py (missing destroy_process_group ([#144734](https://github.com/pytorch/pytorch/pull/144734))
- XFAIL test_save_load_checkpoint ([#144927](https://github.com/pytorch/pytorch/pull/144927))
- [BE][CP] Use run_subtests instead of parametrize ([#143240](https://github.com/pytorch/pytorch/pull/143240))
- [Pipelining] move scale_grads to base class, add docs ([#144833](https://github.com/pytorch/pytorch/pull/144833))
- [DSD][BE] Rewrite some tests to remove `with_comms` ([#143241](https://github.com/pytorch/pytorch/pull/143241))
- Use torch with statement in torch distributed module ([#144951](https://github.com/pytorch/pytorch/pull/144951))
- [Pipelining] Relax scale_grads assert ([#145010](https://github.com/pytorch/pytorch/pull/145010))
- composability test cleanup ([#145011](https://github.com/pytorch/pytorch/pull/145011))
- Make MultiProcContinuousTest timeout configurable ([#145099](https://github.com/pytorch/pytorch/pull/145099))
- PEP585 update - torch/distributed/tensor ([#145141](https://github.com/pytorch/pytorch/pull/145141))
- [DCP] Fix fsspec fsync bug on .finish() ([#144753](https://github.com/pytorch/pytorch/pull/144753))
- PEP585 update - torch/distributed/fsdp ([#145162](https://github.com/pytorch/pytorch/pull/145162))
- PEP585 update - torch/distributed ([#145164](https://github.com/pytorch/pytorch/pull/145164))
- PEP585 update - torch/testing ([#145200](https://github.com/pytorch/pytorch/pull/145200))
- PEP585 update - torch/distributed ([#145164](https://github.com/pytorch/pytorch/pull/145164))
- [CI][CUDA][Distributed][FSDP] Remove hardcoded world size of 2  ([#145195](https://github.com/pytorch/pytorch/pull/145195))
- [cp] override compute_log_sumexp to True for aten._scaled_dot_product_efficient_attention.default if False ([#145421](https://github.com/pytorch/pytorch/pull/145421))
- Improve torchrun documentation ([#144354](https://github.com/pytorch/pytorch/pull/144354))
- [BE][Ez]: FURB148 - remove useless enumerate calls ([#145619](https://github.com/pytorch/pytorch/pull/145619))
- Remove det_singular OpInfo ([#145533](https://github.com/pytorch/pytorch/pull/145533))
- functional compiled autograd ([#144707](https://github.com/pytorch/pytorch/pull/144707))
- Updates NCCL user buffer registration test for NCCL 2.24.3 ([#145285](https://github.com/pytorch/pytorch/pull/145285))
- [OSS] Add no dist as an argument to DCP top level apis ([#145754](https://github.com/pytorch/pytorch/pull/145754))
- [PGNCCL] Correct some ifdef's ([#145893](https://github.com/pytorch/pytorch/pull/145893))
- [async-TP] Fix scheduling in matmul+reduce-scatter for 2 ranks ([#145846](https://github.com/pytorch/pytorch/pull/145846))
- [OSS] Add kwargs to fsspec reader/writer ([#145845](https://github.com/pytorch/pytorch/pull/145845))
- [PGNCCL] Simplify support macro definition ([#145964](https://github.com/pytorch/pytorch/pull/145964))
- [AsyncMM] re-enable and adapt to cutlass 3.6.0 ([#144011](https://github.com/pytorch/pytorch/pull/144011))
- [CI][Distributed] Fix edge case: One rank case (Rank 0) should get [False, False] ([#146099](https://github.com/pytorch/pytorch/pull/146099))
- Enable ruff F841 on distributed tests ([#146131](https://github.com/pytorch/pytorch/pull/146131))
- Build RowwiseScaledMM.cu for SM89 ([#145676](https://github.com/pytorch/pytorch/pull/145676))
- DeepSpeed github repo move sync ([#146320](https://github.com/pytorch/pytorch/pull/146320))
- dynamo: fsdp throw unimplemented vs attribute error ([#146188](https://github.com/pytorch/pytorch/pull/146188))
- Make regex error catching compatible with Python 3.12+. ([#145945](https://github.com/pytorch/pytorch/pull/145945))
- Refactoring Distributed test cases to be device agnostic [1/n] ([#145222](https://github.com/pytorch/pytorch/pull/145222))
- [1/N][cp][example] flex attention in context parallel (forward pass) ([#145896](https://github.com/pytorch/pytorch/pull/145896))
- [2/N][cp][example] flex attention in context parallel (backward pass) ([#146397](https://github.com/pytorch/pytorch/pull/146397))
- distributed/serialization: add experimental streaming torch.save/load methods ([#146555](https://github.com/pytorch/pytorch/pull/146555))
- [DTensor][conv] add DTensor convolution_backward op support for case where the input Tensor has requires_grad=False ([#142278](https://github.com/pytorch/pytorch/pull/142278))
- Refactoring pipeline parallelism test cases to be device agnostic [1/n] ([#146472](https://github.com/pytorch/pytorch/pull/146472))
- Let _create_cpu_state_dict and _copy_state_dict support DTensor ([#146852](https://github.com/pytorch/pytorch/pull/146852))
- Update test_c10d_object_collectives.py with DistributedTestBase class ([#145056](https://github.com/pytorch/pytorch/pull/145056))
- [DCP] Introduce modules metadata in the storage_meta ([#146654](https://github.com/pytorch/pytorch/pull/146654))
- [DCP] Cache save plans: planner helpers and interface updates ([#147116](https://github.com/pytorch/pytorch/pull/147116))
- Add fqn_modifier at loading_state_dict and unit test ([#146557](https://github.com/pytorch/pytorch/pull/146557))
- [FSDP2] Simplify shard_placement_fn in test ([#146847](https://github.com/pytorch/pytorch/pull/146847))
- [OSS] Update FileSystem methods to properly handle a string argument ([#145751](https://github.com/pytorch/pytorch/pull/145751))
- [fr][fix] Split MatchState and dynamic info for fr analysis downstream ([#147439](https://github.com/pytorch/pytorch/pull/147439))
- Add super().setUp() to some test cases ([#147651](https://github.com/pytorch/pytorch/pull/147651))
- Enable ASAN in CUDA tests ([#147512](https://github.com/pytorch/pytorch/pull/147512))
- [DCP] Cache save plans in default planner ([#147343](https://github.com/pytorch/pytorch/pull/147343))
- Remove HuggingFace reader and writer from __init__.py ([#148030](https://github.com/pytorch/pytorch/pull/148030))
- Clean temporary directory at exit ([#147813](https://github.com/pytorch/pytorch/pull/147813))
- [BE][Ez]: Remove extra copy in dtensor parallel loss ([#148096](https://github.com/pytorch/pytorch/pull/148096))
- [BE][PYFMT] migrate PYFMT for `torch.{distributed,distributions}` to `ruff format` ([#144547](https://github.com/pytorch/pytorch/pull/144547))
- Build a storage reader/writer to write checkpoints in HF format ([#148089](https://github.com/pytorch/pytorch/pull/148089))
- [DTensor][Test] Add a test to demonstrate current dtensor view behavior if redistribution happens ([#148015](https://github.com/pytorch/pytorch/pull/148015))
- HSDP custom hook UTs are multi-threaded - can't set device rank ([#148099](https://github.com/pytorch/pytorch/pull/148099))
- [Dyamo] Replace unimplemented with unimplemented_v2 for variables/distributed ([#148500](https://github.com/pytorch/pytorch/pull/148500))
- [CUDA Graphs][NCCL] Set event queries to happen under thread-local mode in `ProcessGroupNCCL.cpp` ([#148594](https://github.com/pytorch/pytorch/pull/148594))
- Skip distributed subprocess test internally as they don't work ([#148909](https://github.com/pytorch/pytorch/pull/148909))
- Fix DCP link ([#148974](https://github.com/pytorch/pytorch/pull/148974))
### security

### Removed because not released yet
- [SymmetricMemory] introduce multimem_all_gather ([#142810](https://github.com/pytorch/pytorch/pull/142810))
- [SymmetricMemory] fix an issue where rendezvous is performed with wrong device context when torch.cuda.set_device() is not callled ([#144886](https://github.com/pytorch/pytorch/pull/144886))
- Support SymmetricMemory's signaling kernels on sm60 and sm70 ([#146308](https://github.com/pytorch/pytorch/pull/146308))
- Fix type stubs for SymmetricMemory ([#146310](https://github.com/pytorch/pytorch/pull/146310))
- Add support for non functional collectives under FakeTensorMode and fake_pg for memory tracking ([#147566](https://github.com/pytorch/pytorch/pull/147566))
- [DDP] Temporarily disable comm mem ([#147663](https://github.com/pytorch/pytorch/pull/147663))
- [DDP] Use NCCL allocated memory for gradient bucket ([#146589](https://github.com/pytorch/pytorch/pull/146589))
- [c10d] Restrict use condition of NCCL mem pool ([#147764](https://github.com/pytorch/pytorch/pull/147764))
