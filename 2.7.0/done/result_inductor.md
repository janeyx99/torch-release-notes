## inductor

### bc breaking

 - New interface for `GraphTransformObserver` to enable Node Level provenance tracking. We now track a mapping between the nodes in the pre-grad and post-grad graph. See the issue for an example frontend to visualize the transformations. To update your `GraphTransformObserver` subclasses, instead of overriding `on_node_creation` and `on_node_erase`, there are new functions `get_node_creation_hook`, `get_node_erase_hook`, `get_node_replace_hook` and `get_deepcopy_hook`. These are registered on the `GraphModule` member of the `GraphTransformObserver` upon entry and exit of a `with` block ([#144277](https://github.com/pytorch/pytorch/pull/144277)).
**Before:**
```python
class MyPrintObserver(GraphTransformObserver):
    def on_node_creation(self, node: torch.fx.Node):
        print(node)
```
**After:**
```python
class MyPrintObserver(GraphTransformObserver):
    def get_node_creation_hook(self):
        def hook(node: torch.fx.Node):
            print(node)
        return hook
```

### deprecations

 - Drop support for Triton versions without ASTSource (around Triton version 2.2.0) ([#143817](https://github.com/pytorch/pytorch/pull/143817)).

### new features

- Enable non power-of-2 `head_dim` for FlexAttention ([#133495](https://github.com/pytorch/pytorch/pull/133495)).
- Add FlexAttention kernel parameter tuning options: `num_warps` and `num_stages` ([#139639](https://github.com/pytorch/pytorch/pull/139639)).
- Support vectorization for score and mask in FlexAttention CPU ([#143638](https://github.com/pytorch/pytorch/pull/143638)).
- `ConfigFuzzer`: a new debugging tool designed to fuzz Torch compile configurations. Given a test function, it will identify combinations of configs that throw errors during compilation and execution ([#139736](https://github.com/pytorch/pytorch/pull/139736)) ([#145565](https://github.com/pytorch/pytorch/pull/145565)).
- Support fusion of pointwise ops into Template Prologues. `TORCHINDUCTOR_PROLOGUE_FUSION` enables this feature ([#147008](https://github.com/pytorch/pytorch/pull/147008)).
- Add instantiation level for generating configs in the CUTLASS backend. Set `TORCHINDUCTOR_CUTLASS_INSTANTIATION_LEVEL`. Consult config.py for information ([#146230](https://github.com/pytorch/pytorch/pull/146230)).
- Add L2 Swizzle config for CUTLASS backend: `cuda.cutlass_max_profiling_swizzle_options` ([#146088](https://github.com/pytorch/pytorch/pull/146088)).
- Emit a CMakeLists.txt when `package_cpp_only` is specified in AOTI ([#143352](https://github.com/pytorch/pytorch/pull/143352)).
- One Dynamo graph can now map to multiple inductor graphs with different `graph_partition` functions. Set the `graph_partition` in inductor config to enable ([#147038](https://github.com/pytorch/pytorch/pull/147038)).

### improvements

- Add profiling support for codegened CPU FlexAttention kernels ([#145894](https://github.com/pytorch/pytorch/pull/145894)).
- Other FlexAttention improvements: ([#147765](https://github.com/pytorch/pytorch/pull/147765)) ([#147435](https://github.com/pytorch/pytorch/pull/147435)) ([#147010](https://github.com/pytorch/pytorch/pull/147010)) ([#146657](https://github.com/pytorch/pytorch/pull/146657)) ([#145059](https://github.com/pytorch/pytorch/pull/145059)) ([#144938](https://github.com/pytorch/pytorch/pull/144938)) ([#143299](https://github.com/pytorch/pytorch/pull/143299)) ([#142281](https://github.com/pytorch/pytorch/pull/142281)) ([#147918](https://github.com/pytorch/pytorch/pull/147918)) ([#148857](https://github.com/pytorch/pytorch/pull/148857)).
- Add Inductor support for non-power-of-2 cooperative RSPLIT ([#145689](https://github.com/pytorch/pytorch/pull/145689)).
- Add Cutlass support for runtime param choices, starting with `swizzle` ([#147223](https://github.com/pytorch/pytorch/pull/147223)).
- Make Inductor cpp backend enable_floating_point_contract_flag take string. Previously, the only options were "on" or "off". Now the value of `INDUCTOR_CPP_ENABLE_FLOATING_POINT_CONTRACT_FLAG` will be passed to `ffp-contract` ([#143450](https://github.com/pytorch/pytorch/pull/143450)).
- Add upcasting FP16/BF16 math reductions to FP32 in Triton ([#141052](https://github.com/pytorch/pytorch/pull/141052)).
- Support for more types of async_compile pools. Set variable `TORCHINDUCTOR_WORKER_START` to one of "subprocess", "fork", or "spawn" ([#144491](https://github.com/pytorch/pytorch/pull/144491)).
- Create a new benchmarker to replace Triton's `do_bench` ([#133058](https://github.com/pytorch/pytorch/pull/133058)).
- Inplace-padding support for cpp-wrapper ([#145325](https://github.com/pytorch/pytorch/pull/145325)).
- New environment variables for `emulate_precision_casts`: `TORCHINDUCTOR_EMULATE_PRECISION_CASTS` ([#145948](https://github.com/pytorch/pytorch/pull/145948)).
- New environment variables to filter cutlass kernels: `TORCHINDUCTOR_CUTLASS_ALLOWLIST` and `TORCHINDUCTOR_CUTLASS_DENYLIST` ([#148161](https://github.com/pytorch/pytorch/pull/148161)).
- Add option to disable runtime scalar assertions: `TORCHINDUCTOR_SCALAR_ASSERTS` ([#146462](https://github.com/pytorch/pytorch/pull/146462)).
- Add new inductor configs to compiler bisector: `layout_optimization` and `comprehensive_padding` ([#148450](https://github.com/pytorch/pytorch/pull/148450)).
- Add an option to skip optimizing generated wrapper code. Set `AOT_INDUCTOR_COMPILE_WRAPPER_WITH_O0=1` ([#144866](https://github.com/pytorch/pytorch/pull/144866)).
- Support dynamic shape constraints in Export ([#146044](https://github.com/pytorch/pytorch/pull/146044)).
- Handle MLIR scf.yield more accurately in user Triton code ([#147762](https://github.com/pytorch/pytorch/pull/147762)).
- Add a global_scratch arg to support Triton 3.3 ([#148051](https://github.com/pytorch/pytorch/pull/148051)).
- Removed an unnecessarily struct runtime alignment assertion, allowing more flexible use cases of AOTI ([#143236](https://github.com/pytorch/pytorch/pull/143236)).
- Support `_int_mm` in AOTI ([#144571](https://github.com/pytorch/pytorch/pull/144571)).
- Support AOTI + CUDAGraphs when calling from Python ([#148601](https://github.com/pytorch/pytorch/pull/148601)).
- New post grad pass to remove `torch.ops.aten._assert_tensor_metadata.default` for AOTI ([#145028](https://github.com/pytorch/pytorch/pull/145028)).
- Support basic TorchBind in `aot_compile` and `aoti_compile_and_package` ([#148506](https://github.com/pytorch/pytorch/pull/148506)).
- Add top level tlparse logging for AOTI ([#147760](https://github.com/pytorch/pytorch/pull/147760))

### bug fixes

- Fix a bug where the options dictionary on `torch.compile` calls was ignored ([#145131](https://github.com/pytorch/pytorch/pull/145131)).
- Inductor now supports `nanj` in cpp wrapper CPU ([#144064](https://github.com/pytorch/pytorch/pull/144064)).
- Fix a bug in the `fractional_max_pool` lowering in Inductor ([#144395](https://github.com/pytorch/pytorch/pull/144395)).
- FlexAttention: Fix a few more symbolic shape issues ([#142816](https://github.com/pytorch/pytorch/pull/142816)).
- Fix a bug in `associative_scan` ([#143048](https://github.com/pytorch/pytorch/pull/143048)).
- Fix the Index Put lowering with same input of self and values ([#139366](https://github.com/pytorch/pytorch/pull/139366)).
- Fix a bug in `torch.polygamma(n)` when n == 0 ([#144058](https://github.com/pytorch/pytorch/pull/144058)).
- Fix bug in integer `avg_pool` that was causing 0 rounding ([#144059](https://github.com/pytorch/pytorch/pull/144059)).
- Change `avg_pool` with `uint` to match eager ([#144313](https://github.com/pytorch/pytorch/pull/144313)).
- Fix bug in max-autotune on smaller GPUs (<68 SMs) ([#145133](https://github.com/pytorch/pytorch/pull/145133)).
- Fix bug in `torch.logit` decomposition ([#145576](https://github.com/pytorch/pytorch/pull/145576)).
- Fix bug in the strides when lowering custom op ([#148367](https://github.com/pytorch/pytorch/pull/148367)).
- Update triton support to account for changes in AttrsDescriptor ([#145051](https://github.com/pytorch/pytorch/pull/145051)) ([#145348](https://github.com/pytorch/pytorch/pull/145348)) ([#145575](https://github.com/pytorch/pytorch/pull/145575)) ([#145583](https://github.com/pytorch/pytorch/pull/145583)) ([#145515](https://github.com/pytorch/pytorch/pull/145515)).
- Fix bug where the `benchmark_harness` isn't generated, but is called in some cases ([#145532](https://github.com/pytorch/pytorch/pull/145532)).
- Make sure not using cpp wrapper when setting nvtx training annotation ([#145538](https://github.com/pytorch/pytorch/pull/145538)).
- Fix bug where `SVE256` features were run on `SVE128` systems ([#146207](https://github.com/pytorch/pytorch/pull/146207)).
- Fix an unaligned memory access issue in `mm_template` ([#146293](https://github.com/pytorch/pytorch/pull/146293)).
- Fix intermediate debug information with `cpp_wrapper` ([#145527](https://github.com/pytorch/pytorch/pull/145527)).
- Fix bug where inductor was codegen-ing wrong shapes for bucketize when it was fused as an epilogue ([#148769](https://github.com/pytorch/pytorch/pull/148769)).
- Fix bug in AOTI one-pass codegen when max-autotune is turned on ([#143098](https://github.com/pytorch/pytorch/pull/143098)).
- Fix a memory leak in package `AOTIModelPackageLoaderPybind::boxed_run` ([#146100](https://github.com/pytorch/pytorch/pull/146100)).
- Fix `None` and `equal_to_1` arguments issue in Triton kernel generated by AOTI ([#148102](https://github.com/pytorch/pytorch/pull/148102))

### performance

- Support for Arm Neon and SVE support for FP32 Gemm Wrapper ([#144327](https://github.com/pytorch/pytorch/pull/144327)).
- New GEMM kernel: `persistent_tma` ([#142101](https://github.com/pytorch/pytorch/pull/142101)).
- Enable CPP Grouped GEMM Template ([#143796](https://github.com/pytorch/pytorch/pull/143796)).
- Auto-tuning support for i8 x i8 -> i32 GEMM kernel on AMX ISA ([#143187](https://github.com/pytorch/pytorch/pull/143187)).
- Add new GEMM templates for CPU AVX512: `_weight_int4pack_mm_for_cpu` ([#146756](https://github.com/pytorch/pytorch/pull/146756)).
- Fuse `SmoothQuant` int8 linear pattern ([#142036](https://github.com/pytorch/pytorch/pull/142036)).
- Add torchao da8w8 pattern with symmetric quantized activations and weights ([#142110](https://github.com/pytorch/pytorch/pull/142110)).
- Support tiling reduction dimensions: Instead of having a single reduction dimension called "r", we can now support 2D reductions with "r0_" and "r1_" dimensions. 2D reductions generate two nested loops, with different block pointer advancements in each loop body ([#137243](https://github.com/pytorch/pytorch/pull/137243)).
- New config to skip L1 cache for single-use buffers in triton codegen ([#143115](https://github.com/pytorch/pytorch/pull/143115)).
- Implement `max_pool2d_with_indices` as a reduction for large window sizes ([#147876](https://github.com/pytorch/pytorch/pull/147876)).
- Optimize the heuristics of outer loop fusion in Inductor CPU backend ([#147523](https://github.com/pytorch/pytorch/pull/147523)).
- Support parallel reduction for GroupNorm in Inductor CPU backend ([#144020](https://github.com/pytorch/pytorch/pull/144020)).
- Add support for online softmax. Online softmax uses a customized reduction to compute max and sum at the same time by accessing the data in one pass ([#127011](https://github.com/pytorch/pytorch/pull/127011)).
- Add ROCm specific matmul tuning parameters ([#148437](https://github.com/pytorch/pytorch/pull/148437)).

### documentation

- Spruce up docs for `emulate_precision_casts` ([#145579](https://github.com/pytorch/pytorch/pull/145579)).
- Minor fixes to export and AOTI docs ([#144513](https://github.com/pytorch/pytorch/pull/144513)).
- Update AOTI tutorial ([#143390](https://github.com/pytorch/pytorch/pull/143390)).
- `inductor.config.descriptive_names = False` is no longer a suggested option ([#145523](https://github.com/pytorch/pytorch/pull/145523)).

### developers

- Support for export to unwrap/wrap subclasses AOT, resolves UX issue in torchao where users had to manually unwrap their subclasses before calling export ([#141941](https://github.com/pytorch/pytorch/pull/141941)).
- Autotuning logs will now show up in `TORCH_LOG`s under the name "autotuning" ([#147222](https://github.com/pytorch/pytorch/pull/147222)).
- Replace `set` by `OrderedSet`: only use OrderedSet in the Inductor codebase ([#138466](https://github.com/pytorch/pytorch/pull/138466)).
- Now MPS is considered a `GPU_TYPE` ([#143634](https://github.com/pytorch/pytorch/pull/143634)).
- Separate unary post op fusion and lowering for `qlinear` ([#143903](https://github.com/pytorch/pytorch/pull/143903)).
- New classes to help with kernel memory analysis in heuristics ([#142026](https://github.com/pytorch/pytorch/pull/142026)).
- Move ir_pre_fusion.txt and ir_post_fusion.txt from `TORCH_COMPILE_DEBUG` to TORCH_LOGS. For example, `TORCH_LOGS="+ir_pre_fusion"` ([#147248](https://github.com/pytorch/pytorch/pull/147248)).
- Implement `deepcopy` for AOTICompiledModel ([#145423](https://github.com/pytorch/pytorch/pull/145423))
