# PyTorch 2.7.0 Release Notes
- [Highlights](#highlights)
- [Backwards Incompatible Changes](#backwards-incompatible-changes)
- [Deprecations](#deprecations)
- [New Features](#new-features)
- [Improvements](#improvements)
- [Bug fixes](#bug-fixes)
- [Performance](#performance)
- [Documentation](#documentation)
- [Developers](#developers)

# Highlights
<table>
  <tr>
   <td><strong>Beta</strong>
   </td>
   <td><strong>Prototype</strong>
   </td>
  </tr>
  <tr>
   <td>Torch.Compile support for Torch Function Modes
   </td>
   <td>NVIDIA Blackwell Architecture Support
   </td>
  </tr>
  <tr>
   <td>Mega Cache
   </td>
   <td>PyTorch Native Context Parallel
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Enhancing Intel GPU Acceleration
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>FlexAttention LLM <span style="text-decoration:underline;">first token processing</span> on X86 CPUs 
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>FlexAttention LLM <span style="text-decoration:underline;">throughput mode optimization</span> on X86 CPUs
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Foreach Map
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Flex Attention for Inference
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Prologue Fusion Support in Inductor
   </td>
  </tr>
</table>

For more details about these highlighted features, you can look at the [release blogpost](https://pytorch.org/blog/pytorch2-7/).
Below are the full release notes for this release.

# Backwards Incompatible Changes

### Dropped support for Triton < 2.2.0. Removed Support for CUDA 12.4, Anaconda in CI/CD.
- Removed CUDA 12.4 support in CI/CD in favor of 12.8 (#148895, #142856, #144118, #145566, #145844, #148602, #143076, #148717)
- Removed Anaconda support in CI/CD (#144870, #145015, #147792)
- Dropped support for Triton < 2.2.0 (versions without ASTSource) (#143817)

### Change `torch.Tensor.new_tensor()` to be on the given Tensor's device by default (#144958)

This function was always creating the new Tensor on the "cpu" device and will now use the same device as the current Tensor object. This behavior is now consistent with other `.new_*` methods.

### Use Manylinux 2.28 and CXX11_ABI=1 for future released Linux wheel builds. 
With Migration to manylinux_2_28 (AlmaLinux 8 based), we can no longer support OS distros with glibc2_26. These include popular Amazon Linux 2 and CentOS 7. (#143423, #146200, #148028, #148135, #148195, #148129)

### `torch.onnx.dynamo_export` now uses the ExportedProgram logic path (#137296)

Users using the `torch.onnx.dynamo_export` API may see some `ExportOptions` become
unsupported due to an internal switch to use `torch.onnx.export(..., dynamo=True)`: `diagnostic_options`, `fake_context` and `onnx_registry` are removed/ignored by `ExportOptions`. Only `dynamic_shapes` is retained.

Users should move to use the `dynamo=True` option on `torch.onnx.export` as
`torch.onnx.dynamo_export` is now deprecated. Leverage the [`dynamic_shapes`](https://pytorch.org/docs/stable/export.html#torch.export.export) argument in `torch.onnx.export` for specifying dynamic shapes on the model.

Version 2.6.0

```python
torch.onnx.dynamo_export(model, *args, **kwargs)
```

Version 2.7.0

```python
torch.onnx.export(model, args, kwargs=kwargs, dynamo=True)
```

### Finish deprecation of `LRScheduler.print_lr()` along with the `verbose` kwarg to the LRScheduler constructor. (#147301)

Both APIs have been deprecated since 2.2. Please use `LRScheduler.get_last_lr()` to access the learning rate instead.`print_lr` and `verbose` were confusing, not properly documented and were little used, as described in #99270, so we deprecated them in 2.2. Now, we complete the deprecation by removing them completely. To access and print the learning rate of a LRScheduler:

Version 2.6.0
```python
optim = ...
lrsched = torch.optim.lr_scheduler.ReduceLROnPlateau(optim, verbose=True)
// lrsched will internally call print_lr() and print the learning rate      
```

Version 2.7.0
```python
optim = ...
lrsched = torch.optim.lr_scheduler.ReduceLROnPlateau(optim)
print(lrsched.get_last_lr())
```

### libtorch_python.so symbols are now invisible by default on all platforms except Apple (#142214)
Previously, the symbols in libtorch_python.so were exposed with default visibility. We have transitioned to being more intentional about what we expose as public symbols for our python API in C++. After #142214, public symbols will be marked explicitly while everything else will be hidden. Some extensions using private symbols will see linker failures with this change.

### Please use `torch.export.export` instead of `capture_pre_autograd_graph` to export the model for pytorch 2 export quantization (#139505)

`capture_pre_autograd_graph` was a temporary API in `torch.export`. Since now we have a better longer term API: `export` available, we can deprecate it.

Version 2.6.0
```python
from torch._export import capture_pre_autograd_graph
from torch.ao.quantization.quantize_pt2e import prepare_pt2e
from torch.ao.quantization.quantizer.xnnpack_quantizer import (
    XNNPACKQuantizer,
    get_symmetric_quantization_config,
)
quantizer = XNNPACKQuantizer().set_global(
    get_symmetric_quantization_config()
)
m = capture_pre_autograd_graph(m, *example_inputs)
m = prepare_pt2e(m, quantizer)
```

Version 2.7.0
```python
from torch.export import export
from torch.ao.quantization.quantize_pt2e import prepare_pt2e
# please get xnnpack quantizer from executorch (https://github.com/pytorch/executorch/)
from executorch.backends.xnnpack.quantizer.xnnpack_quantizer import (
    XNNPACKQuantizer,
    get_symmetric_quantization_config,
)
quantizer = XNNPACKQuantizer().set_global(
    get_symmetric_quantization_config()
)
m = export(m, *example_inputs)
m = prepare_pt2e(m, quantizer)
```

### New interface for `torch.fx.passes.graph_transform_observer.GraphTransformObserver` to enable Node Level provenance tracking (#144277)
We now track a mapping between the nodes in the pre-grad and post-grad graph. See the issue for an example frontend to visualize the transformations. To update your `GraphTransformObserver` subclasses, instead of overriding `on_node_creation` and `on_node_erase`, there are new functions `get_node_creation_hook`, `get_node_erase_hook`, `get_node_replace_hook` and `get_deepcopy_hook`. These are registered on the `GraphModule` member of the `GraphTransformObserver` upon entry and exit of a `with` block

Version 2.6.0

```python
class MyPrintObserver(GraphTransformObserver):
    def on_node_creation(self, node: torch.fx.Node):
        print(node)
```
Version 2.7.0
```python
class MyPrintObserver(GraphTransformObserver):
    def get_node_creation_hook(self):
        def hook(node: torch.fx.Node):
            print(node)
        return hook
```

### `torch.ao.quantization.pt2e.graph_utils.get_control_flow_submodules` is no longer public (#141612)
We are planning to make all functions under `torch.ao.quantization.pt2e.graph_utils` private. This update marks `get_control_flow_submodules` as a private API. If you have to or want to continue using `get_control_flow_submodules`, please make a private call by using `_get_control_flow_submodules`.

**Example:**
Version 2.6:
```python
>>> from torch.ao.quantization.pt2e.graph_utils import get_control_flow_submodules
  ```

Version 2.7:
```python
>>> from torch.ao.quantization.pt2e.graph_utils import get_control_flow_submodules
ImportError: cannot import name 'get_control_flow_submodules' from 'torch.ao.quantization.pt2e.graph_utils'
>>> from torch.ao.quantization.pt2e.graph_utils import _get_control_flow_submodules  # Note: Use _get_control_flow_submodules for private access
```

# Deprecations

### `torch.onnx.dynamo_export` is deprecated (#146425, #146639, #146923)

Users should use the `dynamo=True` option on `torch.onnx.export`.

Version 2.6.0

```python
torch.onnx.dynamo_export(model, *args, **kwargs)
```

Version 2.7.0

```python
torch.onnx.export(model, args, kwargs=kwargs, dynamo=True)
```

### `XNNPACKQuantizer` is deprecated in PyTorch and moved to ExecuTorch, please use it from `executorch.backends.xnnpack.quantizer.xnnpack_quantizer` instead of `torch.ao.quantization.quantizer.xnnpack_quantizer`. (#144940)

`XNNPACKQuantizer` is a quantizer for xnnpack that was added into pytorch/pytorch for initial development. However, as it is not related to our core quantization workflow, we have moved it to ExecuTorch instead. Please use it from `executorch.backends.xnnpack.quantizer.xnnpack_quantizer` instead of `torch.ao.quantization.quantizer.xnnpack_quantizer`.

Version 2.6.0
```python
from torch._export import capture_pre_autograd_graph
from torch.ao.quantization.quantize_pt2e import prepare_pt2e
from torch.ao.quantization.quantizer.xnnpack_quantizer import (
    XNNPACKQuantizer,
    get_symmetric_quantization_config,
)
quantizer = XNNPACKQuantizer().set_global(
    get_symmetric_quantization_config()
)
m = capture_pre_autograd_graph(m, *example_inputs)
m = prepare_pt2e(m, quantizer)
```
Version 2.7.0
```python
# we also updated the export call
from torch.export import export
from torch.ao.quantization.quantize_pt2e import prepare_pt2e
# please get xnnpack quantizer from executorch (https://github.com/pytorch/executorch/)
from executorch.backends.xnnpack.quantizer.xnnpack_quantizer import (
    XNNPACKQuantizer,
    get_symmetric_quantization_config,
)
quantizer = XNNPACKQuantizer().set_global(
    get_symmetric_quantization_config()
)
m = export(m, *example_inputs)
m = prepare_pt2e(m, quantizer)
```

# New features
## Release Engineering
- Added support for CUDA 12.8 in CI/CD  (#145567, #145789, #145792, #145765, #146019, #146378, #146957, #147037, #146265, #147607, #148000, #149584)
- Added Python 3.13 and 3.13t support in CI/CD (#144698, #143078, #144697, #143074, #141806, #146614)
- Added aarch64 support for pytorch-triton package (#148768, #148705)
- Added support Windows XPU CI/CD (#148755, #147637, #148313, #143185, #148319, #144316, #144644, #144034, #145255)
- Added support for ROCm MI300 CI/CD (#143673, #145504, #146675, #147904, #145398, #145621, #145829, #145790, #144594)
- Added support for [PEP585](https://peps.python.org/pep-0585/), Type Hinting Generics In Standard Collections (#145707, #145177, #145708, #145342, #145101)
- Added Windows Arm64 Nightly Builds (#139760)

## Python Frontend
- Introduce a new `torch.utils.serialization.config` namespace for all serialization related configurations (#143324)
- Add `torch.serialization.config.save.use_pinned_memory_for_d2h` to speed up `torch.save` when passed gpu devices (#143342)
- Add `torch.utils.serialization.config.load.calculate_storage_offsets` to reduce random reads and significantly improve performance for storage with bad random access performance (#143880)
- Add support for `__torch_function__` handler on dtype arguments, similar to subclass objects (#145085)

## C++ Extensions
- Support libtorch-agnostic extensions with stable torch ABI (#148892, #148832, #148124, #149208, #149052)

## Distributed
#### Context Parallel
- We provided a Context Parallel API (#131351) for users to parallelize `torch.nn.functional.scaled_dot_product_attention` over the sequence dimension. We implemented
  Ring Attention (#131351) and an AllGather-based approach (#132820) where the all-gather is issued before the first local SDPA
  and the subsequent local SDPAs will have to wait until the all-gather completes, and offered a user API (#142093) to select the desired approach. The implementation
  currently supports three SDPA kernels: `SDPBackend.FLASH_ATTENTION`, `SDPBackend.EFFICIENT_ATTENTION`, and `SDPBackend.CUDNN_ATTENTION` (#148537). We also
  verified that our Context Parallel implementation is compatible with other parallelisms and `torch.compile`.
#### c10d
- Implemented ncclCommInitRankScalable (merging #136789) (#144794)
#### Distributed Checkpoint (DCP)
- Cache save plans: to mitigate overhead from planning steps (#147116, #147343)
- Build a storage reader/writer to write checkpoints in HF format (#148089)

## CUDA
- Blackwell support added across native kernels, CUDA math libraries, and `torch.compile` (#145270)
- Make `torch.cuda.gds` APIs public (#147120)

## MPS
- Prototype of torch.compile for Metal (#143893)
- Provide Metal kernel authoring via Python (#148972)

## ROCm
- CK Memory-Efficient Attention (attention bias support) (#147778)
- CK Flash Attention Backend (#143695)
- Enhanced Windows support for PyTorch on ROCm (#148563, #144098)
- Support for gfx1102 arch (Navi33) in wheel builds (#147761)
- hipblaslt rowwise f8 gemm (#144432)

## XPU
- Add AOT Inductor support for Intel GPU (#140269, #140664, #149175)
- Support `torch.compile` on Windows Platform for XPU (#147637, #144316, #149511)
- Support SYCL with `torch.utils.cpp_extension` APIs (#132945)
- Enhance Intel GPU performance on PyTorch 2 Export Post Training Quantization (#136753, #135465,#135337, #135189)
- Enable windows Kineto profiler(#148319)
- Enable TF32 support for XPU based on oneDNN backend (#137570)

## torch.compile
#### Dynamo
- Support tracing `contextlib.contextmanager` in Dynamo (#136033)
- `nonstrict_trace` escape hatch to apply non-strict tracing to difficult-to-compile code (#146367)
- Delayed compile for dynamic shapes (#147983)
- Support tracing generators (#141055)
- Whitelist of source files to apply dynamic shapes to (#147979)
- Support tracing `list` subclasses (#146819)
#### Inductor
- Enable non power-of-2 `head_dim` for FlexAttention (#133495).
- Add FlexAttention kernel parameter tuning options: `num_warps` and `num_stages` (#139639).
- Support vectorization for score and mask in FlexAttention CPU (#143638).
- `ConfigFuzzer`: a new debugging tool designed to fuzz Torch compile configurations. Given a test function, it will identify combinations of configs that throw errors during compilation and execution (#139736) (#145565).
- Support fusion of pointwise ops into Template Prologues. `TORCHINDUCTOR_PROLOGUE_FUSION` enables this feature (#147008).
- Add instantiation level for generating configs in the CUTLASS backend. Set `TORCHINDUCTOR_CUTLASS_INSTANTIATION_LEVEL`. Consult config.py for information (#146230).
- Add L2 Swizzle config for CUTLASS backend: `cuda.cutlass_max_profiling_swizzle_options` (#146088).
- Emit a CMakeLists.txt when `package_cpp_only` is specified in AOTI (#143352).
- One Dynamo graph can now map to multiple inductor graphs with different `graph_partition` functions. Set the `graph_partition` in inductor config to enable (#147038).


## Profiler
- Add overload names to profiler (#143114)
- Enable profiling on all threads via `experimentalConfig` (#143659)

## Quantization
- Enables kernel from KleidAI to run model that was quantized such that weights are in int4 (with symmetric quantization either using channel-wise or group-wise, with the group size being a multiple of 32), while at runtime the activations are dynamically quantized from fp32 to int8 and weights are upcast from int4 to int8 so that int8 matrix multiplication is executed. This dynamic quantization of activations and matrix multiplication is performed inside of function `torch.ops.aten._dyn_quant_matmul_4bit`, while the weights, scaled and optional bias are packed in `torch.ops.aten._dyn_quant_pack_4bit_weight`. To use it on your model you can quantize it using the following example that leverages `torchao`:
```python
from torchao.dtypes import PlainLayout
from torchao.experimental.packed_linear_int8_dynamic_activation_intx_weight_layout import (
    PackedLinearInt8DynamicActivationIntxWeightLayout,
)
from torchao.experimental.quant_api import (
    int8_dynamic_activation_intx_weight,
)
from torchao.quantization.granularity import (
    PerGroup,
    PerRow,
)
from torchao.quantization.quant_api import quantize_
from torchao.quantization.quant_primitives import MappingType
my_model = Model()
quantize_(
    my_model,
    int8_dynamic_activation_intx_weight(
        weight_dtype=torch.int4,
        granularity=PerGroup(32), # PerRow() is also supported
        has_weight_zeros=True, # Should be True
        weight_mapping_type=MappingType.SYMMETRIC_NO_CLIPPING_ERR # MappingType.SYMMETRIC can also be used but increases error
        layout=PackedLinearInt8DynamicActivationIntxWeightLayout(target="aten"),
    ),
)
```

## ONNX
#### `torch.onnx.verification.verify_onnx_program` (#148396, #148706, #148730, #148707)

A new verification API `torch.onnx.verification.verify_onnx_program` can now be used to verify numerical accuracy of the exported ONNX model. Users can use the `compare_intermediates` option to identify any operator that causes numerical discrepancies in intermediate tensors. It is possible to use a tool like [model-explorer](https://github.com/justinchuby/model-explorer-onnx) to visualize the verification results.

- Support custom axis name through `dynamic_shapes` (#146321)
- `torch.onnx.export(dynamo=True)` now optimizes the output model by default (#146187)



# Improvements

## Release Engineering
- Added TorchCache Benchmark tests (#147641, #147688, #147782, #147780, #147781, #147783, #147546)
- Upgrade CD to 6.3 for ROCm (#142152, #142151, #143613)
- Add cufile to a dependency list for CUDA 12.x builds and enable use by default (#145748, #148465, #148137)
- Add support for gfx1102 and gfx12 to ROCm  wheel and libtorch builds (#147761, #148562)

## Python Frontend
- Add support for CPU scalar in `torch.addcmul` (#143264)
- Set `-DPy_LIMITED_API` flag for `py_limited_api=True` cpp_extensions (#145764)
- Add support for serialization for uintx/intx in weights_only (#147500)
- Add warning to `torch.jit.load` (#143403)
- Make record/storage alignment in `torch.save` configurable (#147788)
- Support `with` statement on torch.Stream (#140138)

## Autograd
- Allow `torch.autograd.graph.GradientEdge` as `torch.autograd.backward` outputs #144744
- Implement gradient for the `residuals` of `torch.linalg.lstsq` #148526
- Add deterministic kernel for `reflection_pad2d_backward` (#136241)
- Improve softmax backward pass native CUDA implementation (#145866)
- Improve Pareto frontier plot for AutoAC (#148678)

## Dataloader
- Dataloader distributes tasks to workers as they become available when `in_order` is `False` (#142324)
- Update pin memory related APIs to not pass `device` argument. `device` and `pin_memory_device` are discouraged and will be deprecated in the future. (#131858)

## Linear Algebra
- Improve dim argument validation for empty inputs for `torch.cum{min,max}`. (#143920)
- Properly throw an error when trying to sort complex numbers. (#144113)

## Nested Tensor (NJT)
- Support NJT `chunk()` backward on batch dim (#144584)
- Support remaining `*_like` factory functions for NJT (#144889)
- Improve `matmul` with NJTs via backward support and composition with dense tensors (#144587, #146405)

## torch.nn
- Add `strict` kwarg to `nn.Module.set_submodule` and fix bug for non dot-delineated strings (#143455)
- Improve input dimensions check for `reflection_pad1d`, `reflection_pad2d` and `reflection_pad3d` (#141670)

## torch.optim
- Refactor AdamW to subclass Adam (#143710, #144972)
- Add support for differentiable LR and weight_decay in SGD, Adam(W) (#143510, #143679, #143726)

## Build Frontend
- Make PyTorch with HomeBrew installed OpenMP (#145870)
- Enable onednn in pytorch for ppc64le architecture (#143743)
- Enable build for Blackwell GPU family (#145436)
- Fix OOM whle building on RasberryPi by sharding codegenerated files (#144364)

## C++ Frontend
- Introduce a new API `isAcceleratorExcluded` (#144959)

## Distributed
#### c10d
- Simplified `abort` and `shutdown` by adding both to `Backend` and `ProcessGroup` objects (#148798)
- Used `new_group` instead of `split_group` on non-CUDA device (#141469)
- Removed `call_guard` in pybind object init of c10d (#143598)
- Enabled coalescing path on XPU and dispatch to XPU tensor barrier if XCCL backend is specified. (#143735)
- Preserved PyWork's Python reference counting when used in functional collectives (#146376)
- Enabled soft fail bind when agent store active inside TCPStore (#147465)
- Made `getDefaultBackend` more fault tolerant (#148596)
#### DistributedDataParallel (DDP)
- Added `init_sync` option to control collectives during initialization (#142824)
- Decoupled python reducer from compilation mode (#147123)
#### FullyShardedDataParallel2 (FSDP2)
- Clamp `reduce_dtype` in lazy init (#143297)
- Enabled FSDP2 on XPU device (#143737)
- Made post-backward condition more robust (#144781)
- Enabled MTIA device in FSDP2 library code (#145842)
- Avoided resetting version counter of all_gather_output in inference_mode (#146709)
- Supported ignoring parameters in FSDP2 (#146631)
- Enabled FSDP tests on XPU device (#147518)
- Enabled FSDP2 on HPU device (#148667)
#### DTensor
- Added `aten.amin/amax` to `linear_reduction_strategy` (#143747)
- Added `src_data_rank` to `distribute_tensor` API (#143883)
- Added strategy for `_scaled_mm` (#143760)
- Added `aten.view.dtype` op support (#144404)
- Enabled sharding prop to handle cross mesh computation (#147869)
- Added CuDNN SDPA op support to DTensor (#148537)
- Optimized `shard_dim_alltoall` to use `alltoall_single` (#148868)
- Deprecated `_shard_tensor` to use `src_data_rank=None` (#144171)
- Added pointwise ops strategy for `aten.minimum` (#145816)
#### TensorParallel
- Propagated `src_data_rank` kwarg in TP API (#144005)
#### Torch Elastic
- Added kill logic for current process when killing a worker (#141060)
- Made `etcd_rendezvous` publicly importable (#145396)
- Exposed the rendezvous keepalive arguments (#145228)
#### Pipelining
- Added `generate_stage_to_rank_mapping` utility (#146193)
- Removed `stage_index_to_group_rank` from schedule (#146217)


## CPU
#### General
- Implement blend operation for float, double, int in VEC ATen backend for SVE (#146479)
- Upgrade submodule oneDNN to v3.7.1 (#148293)
#### x86
- Add support for int8 `brgemm` (#143384)

## CUDA
- Refine CUDA Stream priority (#143849)
- Expose `sharedMemPerMultiprocessor` device property to python (#143119)
- Expose remaining sharedMem `cudaDeviceProps` to python (#143226)
- Add range check for embedding_bag on input `index >= 0` of cuda device (#140791)
- Fix linter warnings (#147386)
- Change behavior of pinning memory so it does not init a cuda context if one is not already present (#145752, #149033)
- Add cutlass kernel for rowwise scaled mm on SM 10.0 (blackwell) (#148421)
- Add `get_stream_from_external` API for CUDA backend (#143799)
- Update cuDNN-frontend submodule to 1.10.0, used by cuDNN convolution and SDPA integrations (#145780)

## MPS
- Adding support to MPS for operators: `angle`, `entr`, `spherical_bessel_j0`,`xlog1py`,` sinc`,`round.decimals`, `linalg.det`,` cholesky.ex`,` bilineard2d_aa`,`linalg.solve`, `zeta`, `cholesky`, `fused_rms_norm`, `lu_unpack`, `lu_factor_ex`, `slogdet` and `logdet` (#143449, #147948, #146818, #147687, #146539, #147266, #146279, #146799, #145526, #146531, #146465, #145701, #145301, #146681, #144651, #145341, #146771, #147914)
- Extending data type support for `angle` and `atan2` for long type, `torch.special.sinc` to complex, `torch.mm` / `torch.bmm` to integral types (#149017, #146648, #145809, #147526)
- Support `torch.accelerator.synchronize()` on MPS (#143171)
- Add error checking when dispatching kernel (#146458)
- For MPSInductor
  * Fix index generation for transpose (#143973)
  * Fix multi rangevar kernel invocation (#144050)
  * Better error when kernel fails to compile (#144649)
  * Fix large prod and sum reductions (#148975)
  * Adding support to MPSInductor for operators: `gamma`, `zeta`, `sinc`, `spherical_bessel_j0`, `entr` (#145341, #146465, #146539, #147650, #148128)

## ROCm
- Fix TunableOp UTs: Rotating Buffer (#143172)
- Enable *_load_dwordx4 ISA for BFloat16 and Half. (#141397)
- Fix condition for small tensor tuning (#144087)

## XPU
- Enable FP64 GEMM (#140677)
- Enable Sparse CSR support (#144722)
- Improve XPU Stream implemenation(#141123,#141119,#142347)
- Enable XPU for Inductor MM Triton Kernel Benchmark (#148237)
- Align XPU `convolution_backward` output layout between fake tensor and real output tensor (#146880)
- Improve error handling and reporting in CMake files (#149353)
- Refine `torch.xpu.get_device_properties` API error message (#144379)
- Enable `nested_layer_norm` support for XPU (#148593)
- Generalize `is_big_gpu()` check in Inductor (#143491)
- Allow XPU device in sparse compressed tensor factory functions (#147306)

## Profiler
- Add optional flag to profiler to toggle external correlations (#143314)
- Add delimeter in memory vizualizer to show where allocation addr begins (#147461)
- Add last entry to truncated values in Kineto args (#148576)
- Add profiler activity for HPU devices (#148182)
- Add HPU availabilities to profiler (#149115)

## torch.compile
#### Dynamo
- Better tracing support for user-defined `dict` subclasses (#143548)
- Improved graph break messages for some common graph break sites (#146525)
- Improved tracing of exceptions (#146492)
- Remove a number of builtin and third-party modules from `trace_rules.py` skipfiles (#145856)
- Remove some specialized variables for specific third-party classes (e.g. `transformers` `ModelOutput`) (#143567)
- Compiled Autograd dropped annotation requirements for custom autograd functions (#146229, #146720)
#### AOTDispatcher
* Fix a quadratic compile time edge case during training when you have long parallel chains of compute (#145082)
* handle compiling mutations on tangents in custom autograd.Functions (#141131)
* handle compiling buffer input mutations of the form `buffer.copy_(int)` (#141161)
* Fix handling of mutable custom operators in compile when used with `torch.inference_mode` (#147925)
#### Dynamic Shapes
* Better unbacked symint handling for `topk` (#147017)
* dynamic shape support for `interpolate(antialias=True)` backward (#141198)
* Better unbacked symint handling in the partitioner (#143877)
* Support dynamic shape inputs to `nonzer_static` (#146006)
* Improve logging in the symbolic shapes framework (provenance tracking, error messages) (#143378, #146625, #146583, #146532, #145354, #146858, #146939, #146955, #147240,#146413m  #145848, #147836, #146298)
* Simplify and speed up `_compute_symbolic_stride()` (#138844)
* Add max kwarg to `torch._check` (#144471)
* Apply hints to symbol not expr when materializing unbacked tensor intermediates in the partitioner (#144097)
* Add `backed_size_oblivious` config (#148696)
* Add `mark_unbacked` strict mode (#147333, #147342)
#### Decompositions, FakeTensor and meta tensors
Several operator decomps received improvements/bugfixes:
* `torch._refs.tensor` (#143461)
* `torch._refs.mean` (#147188)
* `linspace` (#147997)
* `addmv` (#143792)
New meta tensor implementations for a few pytorch operators:
* `nonzero` (#144727)
* `silu`, `sigmoid`, `_softmax`, `embedding` (#147862)
New fake tensor implementation for a few pytorch operators:
* `unique_consecutive` (#145649)
Several general FakeTensor improvements
* force `UntypedStorage.from_buffer(buf)` to return meta storage under FakeTensorMode (#146642)
* support `meta_tensor.to(device='cpu')` under `fake_mode` (#146729)
#### Inductor
- Add profiling support for codegened CPU FlexAttention kernels (#145894).
- Other FlexAttention improvements: (#147765) (#147435) (#147010) (#146657) (#145059) (#144938) (#143299) (#142281) (#147918) (#148857).
- Add Inductor support for non-power-of-2 cooperative RSPLIT (#145689).
- Remove runtime dependency on packaging (#149125) 
- Add Cutlass support for runtime param choices, starting with `swizzle` (#147223).
- Make Inductor cpp backend enable_floating_point_contract_flag take string. Previously, the only options were "on" or "off". Now the value of `INDUCTOR_CPP_ENABLE_FLOATING_POINT_CONTRACT_FLAG` will be passed to `ffp-contract` (#143450).
- Add upcasting FP16/BF16 math reductions to FP32 in Triton (#141052).
- Support for more types of async_compile pools. Set variable `TORCHINDUCTOR_WORKER_START` to one of "subprocess", "fork", or "spawn" (#144491).
- Create a new benchmarker to replace Triton's `do_bench` (#133058).
- Inplace-padding support for cpp-wrapper (#145325).
- New environment variables for `emulate_precision_casts`: `TORCHINDUCTOR_EMULATE_PRECISION_CASTS` (#145948).
- New environment variables to filter cutlass kernels: `TORCHINDUCTOR_CUTLASS_ALLOWLIST` and `TORCHINDUCTOR_CUTLASS_DENYLIST` (#148161).
- Add option to disable runtime scalar assertions: `TORCHINDUCTOR_SCALAR_ASSERTS` (#146462).
- Add new inductor configs to compiler bisector: `layout_optimization` and `comprehensive_padding` (#148450).
- Add an option to skip optimizing generated wrapper code. Set `AOT_INDUCTOR_COMPILE_WRAPPER_WITH_O0=1` (#144866).
- Support dynamic shape constraints in Export (#146044).
- Handle MLIR scf.yield more accurately in user Triton code (#147762).
- Support Triton 3.3: add a `global_scratch` arg, fix cpp_wrapper (#148051, #149973).
- Removed an unnecessarily struct runtime alignment assertion, allowing more flexible use cases of AOTI (#143236).
- Support `_int_mm` in AOTI (#144571).
- Support AOTI + CUDAGraphs when calling from Python (#148601).
- New post grad pass to remove `torch.ops.aten._assert_tensor_metadata.default` for AOTI (#145028).
- Support basic TorchBind in `aot_compile` and `aoti_compile_and_package` (#148506).
- Add top level tlparse logging for AOTI (#147760)
- Added Inductor dashboard benchmarks  (#144427, #145791, #145654, #145655, #146449, #145683, #141371, #143223)
- Add AOTI shim for `_weight_int4pack_mm_cpu_tensor` (#149031)


## torch.fx
- Fix subgraph rewriter to support matched pattern with no users (#143842)
- Improve error message to include entire GraphModule (#146197, #148090)
- Allow overriding of ShapeProp (#148784)

## torch.export
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

## Quantization
- Add an option `keep_original_weights` in `_lower_to_native_backend` (#141049)
- Handle meta tensors in FX quantization (#144726)
- Add fp8 support to index_cuda (#144747)
- Add the `torch.float8_e8m0fnu` dtype to PyTorch (#147466)
- Improve the performance of 8 bit quantized linear and addition operation on AArch64 by leveraging operations from Arm Compute Library (#148585)
- Enables int8 linear operations to use mkl-dnn when activations, weights and accumulation are all signed 8-bit integers (#139887)

## ONNX
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

## JIT
- Relax type-checks for empty dicts (#147167)

## Lazy Tensor
- Introduce cache clearing APIs for the lazy graph executor (#144489)

## torch.package
- Add support for UntypedStorage tensors (#143930)


# Bug fixes

## Python Frontend
- Fix `torch.lerp` type promotion (#141117)
- Fix memory leak on `torch.Tensor` when both slots and python gc are used (#143203)
- Fix `torch.bfloat16` support for `__cuda_array_interface__`. (#143042)
- Fix rare dispatcher bug for inplace operations that would make the returned `torch.Tensor` incorrect. (#145530)
- Stop using MKL for randomness generation on CPU (#146174)
- Move accelerator detection to use build time (#146098)
- Fix `torch.load` under `FakeTensorMode` to create `FakeTensor` with correct devices (for plain Tensors) (#147786)
- Fix `torch.acos`, `torch.asin`, `torch.atan`, `torch.exp`, `torch.sigmoid`, `torch.div`, for `torch.complex` datatypes on CPU (#134838, #140358, #140391, #140375, #144749)

## Autograd
- Fix `torch.autograd.graph.allow_mutation_on_saved_tensors` for inplace foreach ops #145520
- Fix boundary conditions for `hardswish` backward (#143899)
- Use float data type for Half sum in fallback implementation of `batchnorm` backward on CPU (#147353)
- Fix `torch.compile` + ddp + non-reentrant AC pack hook firing count (#144271)

## Linear Algebra
- Fix workarea compute in `eigh` (#146456)

## Nested Tensor (NJT)
- Fix NJT `min` / `max` backward() for non-ragged reductions (#144583)
- Fix NJT `frexp()` to handle both outputs (#144585)
- Fix NJT `fill.Scalar` for contiguous inputs (#144586)
- Fix inference mode for composite implicit ops without nested-specific kernel (#146633)
- Fix flop counter for SDPA and test (#147032)

## torch.nn
- Fix broken meta function for flex-attention backwards (#146563)

## Build Frontend
- Fix unbalanced `#pragma diagnostic pop` in VecLib (#148354)
- Fix atomic operation compatibility for ARMv8-A (Raspberry Pi 4) by adjusting compilation flags (#148070)
- Make PyTorch buildable by CMake-4.x (#150203)

## C++ Frontend
- Fix Apple Clang ICE when building with -march=armv8.6a (#142879)
- Fix inductor regression on aarch64 neoverse-v1 with gcc10.2 by disabling tree vectorization (#148489)

## Distributed
#### Distibuted Checkpoint (DCP)
- fix dcp gather_object/scatter_object_list (#147675)
#### Distributed (c10d)
- Fixed `CudaEventCache` for dangling references (#144496)
- Make `all-reduce` input contiguous in `distributed.nn.all_reduce` (#144267)
- Removed `Alltoallv` specialization for PyTorch generic `all_to_all` (#145045)
- Added a handle case when remote peer closes connection for TCPStore (#145757)
- Fixed memory leak on shutdown (#145507)
- Fixed an issue where functional collectives don't force fx stride on inputs when compiled (#146467)
- Associated tensor allocation support with NCCL version (#146842)
- Modified API to get device string from device with `torch.device` (#146290)
- Fixed `dist.init_process_group` on windows (#148266)
- Fixed capturability of `isend` and `irecv` (#148462)
#### DistributedStateDict (DSD)
- Fixed `strict=False` case for DDP (#143038)
- Fixed issue when there is a PG without parameters (#147730)
- Fixed the shared parameter mismatch for optimizer state_dict when flattening FQNs are used (#148825)
#### FullyShardedDataParallel2 (FSDP2)
- Rooted fix for FP8 tensor (#143248)
- Added workaround to fix `buffer_dtype` without root parameters (#143989)
- Supported custom all reduce hook across FSDP units (#147114)
- Fixed bug in FSDP wrapped module with zero argument  (#147771)
#### DTensor
- Fixed `torch.distributed._functional_collectives.AsyncCollectiveTensor` for `aten.to`. (#134661)
- Deferred DTensor RNG state sync until first random op call or manual_seed call to support more flexible OffsetBasedRNGTracker init (#147025)
- Fixed `_scaled_dot_product_flash_attention` sharding (#148125)
- Fixed redistribution cost for `all-reduce` (#148761)
#### Pipelining
- Fixed backward_one_chunk when the output of the model is a view (#142237)
- Threw error with ZB and compile (#143599)
- Fixed FSDP+PP stream sync bug (#144535)
- Fixed PP grad scaling (#144352)
- No allowing for num_microbatches > num_stages for single stage schedules (#144702)
- Fixed shape_inference for V-schedules (#147000)

## CPU
#### General
- Use sleef implementation for CPP backend `asinh` codegen (#142360)
#### x86
- Constrain the shape of other tensor for `Conv/Linear` + broadcast `add` fusion (#141759)

## CUDA
- Let `PYTORCH_NO_CUDA_MEMORY_CACHING` has effect only when value is 1 (#145905)
- Fix race condition in cuda initialization (#143238)
- Fix a few 64-bit indexing issues, account for number of threads in `complex128` scan (#143401)
- Fix acquire pattern (correctness with respect to memory model) in topk (#144945)
- `Int64` indexing fix for `UpSampleNearest3D` (#144865)
- Fix printing of the number of GPUs when certain asserts are raised (#146838)
- Update the number of threads in `avg_pool2d` backward for SM 10.0 to prevent runtime crash (#145669)
- Only use `f8f8bf16` rowwise scaled matmul to SM 9.0 (precedes #148421 adding of kernel) (#145728)
- Fix 64-bit indexing for `Upsample2D` (#141923)
- Fix path lookup in `_preload_cuda_deps` (#149808)
- Help support Blackwell: Fix backward launch bounds again for `sm100`, `sm120` (#150640)

## MPS
- Workaround for `gather_out` in MPS backend (#135543)
- Fix fmin/fmax for scalar argument (#143934)
- Fix crash when mm is invoked with mixed dtypes (#143948)
- Fix `torch.add(x,y, alpha=2)` crash (#143949)
- Fix `nllnd_loss_backward` crash with different dtypes (#144170)
- Make sure that MPSStream is usable from C++ (#144559)
- Make MPSProfiler usable from C++ (#144560)
- Fix regression in con-contiguous bitwise ops (#146085)
- Fix lu factor for large tensors with bs\>1 (#146753)
- Ensure 4d input in `_scaled_dot_product_attention_math_mps` (#146623)
- Fix `cholesky_ex` for empty inputs (#147159)
- Fix attention for \>4d tensors (#147545)
- Fix empty placeholder error for smooth l1 loss (#148133)
- Fix sqrt and other for `torch.chalf` (#148285)
- Fix `unary_kernel_strided` logic (#148512)
- Fix scalar to tensors bitshifts (#148686)
- Fix multinomial sampling for non-contiguous tensors (#141515)
- Fix triangular for \>3D tensors (#144545)
- Fix missing autorelease in `lstm_mps` causing leaked memory (#145503)
- Fix missing autoreleasepool around runUniqueGraph to prevent leaks (#145512)
- Workaround rng bug for 5D tensors (#147667)
- Fix Wreorder-init-list (#148839)
- Fix invalid format string in libfmt calls (#148855)
- Fix `c10::metal::log_gamma` correctness on M4 (#145740)
- Fix lu factor for non contiguous tensors (#146279)
- Fix attention `enable_gqa` crash on MPS (#149147)
- Fix dot/mm for conj_tensors (#150157)
- Fix `tril` op not handling infs correctly (#149866)
- In MPSInductor:
  * Fix `min`/`max` reductions over large dims (#149004)
  * Fix argmin/max signatures (#149020)
  * Fix `masked`/`where` for inf values (#144500)
  * Move threadfence to before first read from shared memory, not after (#149437)

## ROCm
- TunableOp use thread-safe getenv functions (#142274)
- fix torch.layer_norm invalid configuration problem when input is large tensor (#144007)
- [Inductor][CK] hackfix for segfault in `addmm` op (#144519)
- Fix `torch.layer_norm` invalid configuration when input is large tensor (#144007)
- Fix `isnan` integer overload errors on MicroSoft STL (#146605)
- Fixes and improvements to CUDA->HIP flag conversion for CPP extensions (#149245)

## XPU
- Fix SDPA dummy log_sum_exmp output to match meta function (#148652)
- Fix memory leak in deconv backward (#144385)
- Add XPU support to `torch.utils._content_store` to accelerate XPU tensor hashing for tensor serialization (#147785)
- Enabling XPU in `OffsetBasedRNGTracker` to unbreak `torch.distributed` (#148360)
- `torch.backends.mkldnn.flags()` CM should not warn (#150358)

## Profiler
- Hide Kineto `step()` for iterative on-demand tracking behind environment variable (#144494)
- Enable CUPTI on Windows (#141454)
- Fix device setting error of other backends in `torch.profiler` (#144237)
- Fix assertion failure in PyTorch profiler (#143940)

## torch.compile
- Do not depend on numpy during `torch._functorch` import (#149683)
#### Dynamo
- Guard on global autocast state (#143592)
- Fix some internal crashes involving undefined names (#144784)
- Multiple silent incorrectness fixes for Compiled Autograd (#144707)
- Fix graph break in FlexAttention when using Compiled Autograd (#144533)
#### Inductor
- Fix a bug where the options dictionary on `torch.compile` calls was ignored (#145131).
- Inductor now supports `nanj` in cpp wrapper CPU (#144064).
- Fix a bug in the `fractional_max_pool` lowering in Inductor (#144395).
- FlexAttention: Fix a few more symbolic shape issues (#142816).
- Fix a bug in `associative_scan` (#143048).
- Fix the Index Put lowering with same input of self and values (#139366).
- Fix a bug in `torch.polygamma(n)` when n == 0 (#144058).
- Fix bug in integer `avg_pool` that was causing 0 rounding (#144059).
- Change `avg_pool` with `uint` to match eager (#144313).
- Fix bug in max-autotune on smaller GPUs (<68 SMs) (#145133).
- Fix bug in `torch.logit` decomposition (#145576).
- Fix bug in the strides when lowering custom op (#148367).
- Update triton support to account for changes in AttrsDescriptor (#145051) (#145348) (#145575) (#145583) (#145515).
- Fix bug where the `benchmark_harness` isn't generated, but is called in some cases (#145532).
- Make sure not using cpp wrapper when setting nvtx training annotation (#145538).
- Fix bug where `SVE256` features were run on `SVE128` systems (#146207).
- Fix an unaligned memory access issue in `mm_template` (#146293).
- Fix intermediate debug information with `cpp_wrapper` (#145527).
- Fix bug where inductor was codegen-ing wrong shapes for bucketize when it was fused as an epilogue (#148769).
- Fix bug in AOTI one-pass codegen when max-autotune is turned on (#143098).
- Fix a memory leak in package `AOTIModelPackageLoaderPybind::boxed_run` (#146100).
- Fix `None` and `equal_to_1` arguments issue in Triton kernel generated by AOTI (#148102)
- Fix backwards compatibility for `AOTIModelPackageLoader()` constructor defaults (#149082)
- Fix blank space break windows file path (#149388)
- Fix inductor windows linker error (#150256)

## torch.fx
- Fix `get_source_partitions` when weights are tied (#142446)
- Prevent DCE of ATen rng nodes (#144319)
- Fix incorrect type comparison (#145449)
- Fix DCE of setitem node (#145714)
- Fix pytree.register_constant to be usable in export (#147533)
- Fix edge case in translation validation bisector (#145414)

## torch.export
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
- Symintify `transpose_` (#149057)

## ONNX
- Support subgraphs with 1+ outputs (#145860)
- Delete `rename_dynamic_shapes_with_model_inputs` (#146002)
- Handle number of outputs in builder (#147164)
- Fix missed None type support in `dynamic_shapes` string cases (#148025)


# Performance

## Release Engineering
- Add perf testing on H100 (#146868, #147947)
## Sparse Frontend
- Remove unnecessary tensor `clone`s throughout codebase (#148159)

## Distributed
#### Distributed Checkpoint (DCP)
- Introduce process based async checkpointing (#147039)
#### c10d
- Changed `ALLOC_BUFFER_SIZE` from 4000 to 4096 to be a power of 2 for TCPStore (#145759)
- Improved IPC tensor release performance by releasing the IpcMutex when deleting the `ExpandableSegments` object and the GIL in WorkNCCL destructor (#148805)

## CPU
#### General
- Simplify vec128 bfloat16/half `fmadds` (#144486)
- Parallelize `sort` (#142391)
#### x86
- Set `prop_kind` to `forward_inference` when grad is not needed for `mkldnn_convolution_pointwise` (#142855)
- Support reduce ops for `add` and `max` (#144065)
- use zero-point to decide `conv` src zp mask (#149473)

## CUDA
- Let `PYTORCH_NO_CUDA_MEMORY_CACHING` has effect only when value is 1 (#145905)
- Fix race condition in cuda initialization (#143238)
- Fix a few 64-bit indexing issues, account for number of threads in `complex128` scan (#143401)
- Fix acquire pattern (correctness with respect to memory model) in topk (#144945)
- `Int64` indexing fix for `UpSampleNearest3D` (#144865)
- Fix printing of the number of GPUs when certain asserts are raised (#146838)
- Update the number of threads in `avg_pool2d` backward for SM 10.0 to prevent runtime crash (#145669)
- Only use `f8f8bf16` rowwise scaled matmul to SM 9.0 (precedes #148421 adding of kernel) (#145728)
- Fix 64-bit indexing for `Upsample2D` (#141923)


## MPS
- Faster integer batched matmul (#147877)
- Implement linear1d as shader (#148154)
- Metal unary kernel for sqrt (#148272)
- Faster unary operations for strided tensors (#148350)
- Introduce strides unary op (#148468)
- Implemented `masked_fill_scalar` as shader (#147369)
- Implement `bilineard2d` as shader (#145581)
- Optimize Cholesky (#145722)
- Speedup interpolation (#148277)

## ROCm
- Improve backwards indexing when stride is not one (#147630)
- Improvements for vectorized elementwise kernels (#143269)
- Skip L1 cache for single-use buffers in tl.load (#143115)
- Improve performance of reduce sum for 3D shapes (#143137)
- Enable `_load_dwordx4` ISA for BFloat16 and Half (#141397)
- Improve reduce sum calculation for low CU count (#141378)
- Tune 3d tensor sums when not using fastest dimension (#146170)
- Optimize the stride one indexing backwards kernel (#146420)
- Use IPT=8 for block radix sort (#147657)
- Improve performance of reduce sum for 3D shapes (#143137)
- change preferred blas lib defaults (#150212)

## XPU
- Optimize SDPA Inference Performance for XPU (#147614, #147612)
- Improve zero-point memory creation (#148640)
- Avoid unnecessary copy when the destination tensor of Matmul is non-contiguous or input is broadcasted  (#144759, #143784)

## torch.compile
#### Dynamo
- Implement dynamic shape guards in C++ (#139899)
- Directly access Python frame locals in guard checks (#140063)
- Misc. Dynamo tracing time improvements (#143066)
#### Inductor
- Support for Arm Neon and SVE support for FP32 Gemm Wrapper (#144327).
- New GEMM kernel: `persistent_tma` (#142101).
- Enable CPP Grouped GEMM Template (#143796).
- Auto-tuning support for i8 x i8 -> i32 GEMM kernel on AMX ISA (#143187).
- Add new GEMM templates for CPU AVX512: `_weight_int4pack_mm_for_cpu` (#146756).
- Fuse `SmoothQuant` int8 linear pattern (#142036).
- Add torchao da8w8 pattern with symmetric quantized activations and weights (#142110).
- Support tiling reduction dimensions: Instead of having a single reduction dimension called "r", we can now support 2D reductions with "r0_" and "r1_" dimensions. 2D reductions generate two nested loops, with different block pointer advancements in each loop body (#137243).
- New config to skip L1 cache for single-use buffers in triton codegen (#143115).
- Implement `max_pool2d_with_indices` as a reduction for large window sizes (#147876).
- Optimize the heuristics of outer loop fusion in Inductor CPU backend (#147523).
- Support parallel reduction for GroupNorm in Inductor CPU backend (#144020).
- Add support for online softmax. Online softmax uses a customized reduction to compute max and sum at the same time by accessing the data in one pass (#127011).
- Add ROCm specific matmul tuning parameters (#148437).

## torch.fx
- Micro-optimization in `Graph.nodes.__iter__` (#144631)
- Micro-optimization in `map_aggregate(immutable_dict)` (#147691)
- Move DCE rand check to import time (#145118)

## Quantization
- Enable fast qlinear static/dynamic path for AArch64 through ACL directly (#148585)
- Improve KleidiAI 4 bit kernel performance (#146476)
- Add NEON implementation for 8 bit quantized embedding bag on AArch64 to improve performance by ~5.5x on Neoverse V1 cores (#147322)


# Documentation

## Python Frontend
- Fix description of `input` in `torch.addbmm()` (#146664)
- fix numpy docs reference (#147697)
- Add `torch.cat` type promotion documentation (#141339)
- Add details `torch.topk` indices stability when duplicate values (#143736)
- Add overloads to `torch.diagonal` documentation (#144214)
- remove incorrect warnings from `torch.{min,max}` documentation (#146725)
- Update addbmm, addmm, addmv and baddbmm description (#146689)
- Fix `torch.max` optional args `dim`, `keepdim` description (#147177)
- Update `torch.bucketize` documentaion (#148400)
- Fix docs recommending inefficient tensor op order (#144270)

## Autograd
- Suppress vmap warning from `torch.autograd.gradcheck` #144287

## Nested Tensor (NJT)
- Update OSS nested tensor docs to focus on NJT (#145402)

## torch.nn
- Add clarification for target types in `CrossEntropyLoss` doc (#145444)

## torch.optim
- Clarify what we mean by decoupled weight decay in the *AdamWs (#144101, #144984)
- Corrected description of AMSGrad algorithm (#142351)

## Build Frontend
- Removing doc references to PRE_CXX11_ABI. (#149756)

## Distributed
#### FullyShardedDataParallel2 (FSDP2)
- Highlighted equivalence of `set_requires_gradient_sync` and `no_sync` (#148715)
#### Distributed (c10d)
- Updated docs for `wait()` (#143305)
- Added comments to the end of Macro for better readability (#144789)
#### DTensor
- Added some documentation for `from_group` API and add a 2D test (#146364)
- Expose the `__create_chunk_list__` in the doc (#144100)
#### DistributedStateDict (DSD)
- Updated the document to mention the limitation of `set_optimizer_state_dict` (#148918)
## Torch Elastic
- Replaced incorrect .. note:: invocations (#142868)
- Fixed the doc string for `record` (#146968)
#### Pipelining
- Updated tutorials and documentation (#143045)

## CUDA
- Correct docs for clock_rate to MHz, fixes #147098 (#147393)

## XPU
- Improve "Getting Started on Intel GPU" hardware requirements and notes(#147802, #148168, #150397)
- Improve SYCL extension, source build and AOT Inductor documentation (#147988, #143476, #149299)
- Update Doc for Intel XPU Profiling (#134515)
- Update CMAKE_PREFIX_PATH for XPU windows README (#148863)

## torch.compile
#### Dynamo
- Remove the suggestion to use `suppress_errors` on compiler error (#146553)
- Automatically generated Dynamo docs (#146736)
#### Inductor
- Spruce up docs for `emulate_precision_casts` (#145579).
- Minor fixes to export and AOTI docs (#144513).
- Update AOTI tutorial (#143390).
- `inductor.config.descriptive_names = False` is no longer a suggested option (#145523).

## torch.fx
- Improve logging for splitter (#143771)
- Update literal typing for torch/fx/graph nodelist (#144650)
- Improve typing for torch/fx/_pytree.py and torch/utils/_pytree.py (#145173)
- Fix minor mistake in docstring of replace_pattern (#147611)

## torch.export
- [Export Programming Model](https://pytorch.org/docs/main/export.programming_model.html): #143546
- Update dynamic shapes docs for `dims()` and suggested fixes parser: #142510
- Clean up docstring for `torch.export.load()`: #141490

## Quantization
- Add torchao docs link to PyTorch libraries (#145412)

## ONNX
- Update TorchDynamo-based ONNX Exporter memory usage example code. (#144139)
- Deprecation message follow up (#147005)
- Expose verification utilities (#148603)


# Developers


## Python Frontend
- Collect packages with importlib in collect_env (#144616)
- added `__add__` and `__mul__` hints to `torch.Size` (#144322)

## Distributed
#### FullyShardedDataParallel2 (FSDP2)
- Enabled the typing of `fully_shard` so that the return value can be chained with typing enabled (#147489)
#### Distributed (c10d)
- Improved the dump mechanism for flight recorder (#143446)
- Added log trace capture enabled or not in flight recorder (#143865)
- Added file flush in file based dumper of flight recorder (#145458)
- Caught c10 error and log message inside monitoring thread (#145413)
- Added an API to get the status/error code at the PG level (#144498)
- Moved record param for init to the right place (#148571)
- Enabled testing generelization for multiple accelerator devices (#139749)
#### TensorParallel
- Added warning when module is distributed twice (#147006)
#### Pipelining
- Improved shape inference debug logging (#144929)

## MPS
- Support includes in metal objects (#145087)
- Context manager to capture Metal commands for debugging/profiling (#144561)

## XPU
- Reduce the binary size of the XPU Windows package (#148313)
- Add Python 3.13 build for XPU (#146614)
- Make XPU Triton build supports manylinux 2.28 (#148195)
- Fix XPU builds inside venv (#150300)

## Benchmark
- Remove old ONNX benchmarks from operator benchmarks (#146325)
- Add option to write operator benchmark output to a JSON (#142809)
- Improve operator benchmark results parsing (#144297)
- Add more operators {`add_`, `addcmul`, `arange`, `baddbmm`, `bmm`, `clamp`, `div`, `div_`, `gelu`, `index_add`, `logical_and`, `mul_`, `sub_`, `topk`, `where`} to operator benchmark (#145625)
- Add cachebench to operator benchmarks for PT2 caching (#147537)

## torch.compile
#### Dynamo
- New internal graph break API that enforces better error messages (#146525)
- Replace internal calls to `torch._dynamo.optimize()` with `torch.compile()` (#142451)
#### Inductor
- Support for export to unwrap/wrap subclasses AOT, resolves UX issue in torchao where users had to manually unwrap their subclasses before calling export (#141941).
- Autotuning logs will now show up in `TORCH_LOG`s under the name "autotuning" (#147222).
- Replace `set` by `OrderedSet`: only use OrderedSet in the Inductor codebase (#138466).
- Now MPS is considered a `GPU_TYPE` (#143634).
- Separate unary post op fusion and lowering for `qlinear` (#143903).
- New classes to help with kernel memory analysis in heuristics (#142026).
- Move ir_pre_fusion.txt and ir_post_fusion.txt from `TORCH_COMPILE_DEBUG` to TORCH_LOGS. For example, `TORCH_LOGS="+ir_pre_fusion"` (#147248).
- Implement `deepcopy` for AOTICompiledModel (#145423)

## torch.fx
- Downgrade some logs (#147538, #145075)
- Refactor immutable collections implementation (#144640)
- Make `fx.node.map_arg()` and `.map_aggregate()` generic (#146248)
