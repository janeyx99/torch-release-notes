
# Release Notes worksheet quantization

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

## quantization
### bc breaking
- Please use `torch.export.export_for_training` instead of `capture_pre_autograd_graph` to export the model for pytorch 2 export quantization (#139505)

`capture_pre_autograd_graph` is a temporary API in torch.export, now we have a better longer term API: `export_for_training` available (starting PyTorch 2.5), we can deprecate it.

```python
# pytorch 2.6
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

# pytorch 2.7
from torch.export import export_for_training
from torch.ao.quantization.quantize_pt2e import prepare_pt2e
# please get xnnpack quantizer from executorch (https://github.com/pytorch/executorch/)
from executorch.backends.xnnpack.quantizer.xnnpack_quantizer import (
    XNNPACKQuantizer,
    get_symmetric_quantization_config,
)
quantizer = XNNPACKQuantizer().set_global(
    get_symmetric_quantization_config()
)
m = export_for_training(m, *example_inputs)
m = prepare_pt2e(m, quantizer)
```

### deprecation
- `XNNPACKQuantizer` is deprecated in pytorch and moved to ExecuTorch, please use it from `executorch.backends.xnnpack.quantizer.xnnpack_quantizer` instead of `torch.ao.quantization.quantizer.xnnpack_quantizer`. (#144940)

`XNNPACKQuantizer` is a quantizer for xnnpack, it was added in pytorch core for initial development, but it's not related to core quantization flow. Now we move it to ExecuTorch instead. Please use it from `executorch.backends.xnnpack.quantizer.xnnpack_quantizer` instead of `torch.ao.quantization.quantizer.xnnpack_quantizer`.

```python
# pytorch 2.6
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

# pytorch 2.7
# we also updated the export call
from torch.export import export_for_training
from torch.ao.quantization.quantize_pt2e import prepare_pt2e
# please get xnnpack quantizer from executorch (https://github.com/pytorch/executorch/)
from executorch.backends.xnnpack.quantizer.xnnpack_quantizer import (
    XNNPACKQuantizer,
    get_symmetric_quantization_config,
)
quantizer = XNNPACKQuantizer().set_global(
    get_symmetric_quantization_config()
)
m = export_for_training(m, *example_inputs)
m = prepare_pt2e(m, quantizer)
```


### new features

- Enables kernel from KleidAI to run model that was quantized such that weights are in int4 (with symmetric quantization either using channel-wise or group-wise, with the group size being a multiple of 32), while at runtime the activations are dynamically quantized from fp32 to int8 and weights are upcast from int4 to int8 so that int8 matrix multiplication is executed. This dynamic quantization of activations and matrix multiplication is performed inside of function `torch.ops.aten._dyn_quant_matmul_4bit`, while the weights, scaled and optional bias are packed in `torch.ops.aten._dyn_quant_pack_4bit_weight`. To use it on your model you can quantize it using the following example that leverages `torchao`:
```
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

### improvements
- Add an option `keep_original_weights` in `_lower_to_native_backend` (#141049)
- Handle meta tensors in FX quantization (#144726)
- Add fp8 support to index_cuda (#144747)
- Add the `torch.float8_e8m0fnu` dtype to PyTorch (#147466)
- Improve the performance of 8 bit quantized linear and addition operation on AArch64 by leveraging operations from Arm Compute Library (#148585)
- Enables int 8 linear operations to use mkl-dnn when activations, weights and accumulation are all signed 8 bit integer (#139887)

### bug fixes
### performance
- Improve KleidiAI 4 bit kernel performance (#146476)
- Add NEON implementation for 8 bit quantized embedding bag on AArch64 to improve performance by ~5.5x on Neoverse V1 cores (#147322)
### docs
- Add torchao docs link to PyTorch libraries (#145412)

### devs
### Untopiced


### not user facing
- Turn on AOTAutogradCache by default on open source ([#141981](https://github.com/pytorch/pytorch/pull/141981))
- Improve implementation of quantized_batch_norm ([#141570](https://github.com/pytorch/pytorch/pull/141570))
- Fix unused Python variables in test/[a-d]* ([#134665](https://github.com/pytorch/pytorch/pull/134665))
- Fix unused Python variables in test/[e-z]* ([#136964](https://github.com/pytorch/pytorch/pull/136964))
- remove allow-untyped-defs from torch/ao/quantization/experimental/fake_quantize_function.py ([#143582](https://github.com/pytorch/pytorch/pull/143582))
- remove allow-untyped-defs from torch/ao/quantization/experimental/APoT_tensor.py ([#143601](https://github.com/pytorch/pytorch/pull/143601))
- Enable ruff's unused variable checking everywhere in pytorch ([#136965](https://github.com/pytorch/pytorch/pull/136965))
- Add support for prototype affine quantization in pt2e flow ([#141421](https://github.com/pytorch/pytorch/pull/141421))
- remove allow-untyped-defs from torch/ao/__init__.py ([#143604](https://github.com/pytorch/pytorch/pull/143604))
- [Codemod][AddExplicitStrictExportArg] caffe2/test ([#143688](https://github.com/pytorch/pytorch/pull/143688))
- remove allow-untyped-defs from ao/nn/qat/dynamic/modules/linear.py ([#143919](https://github.com/pytorch/pytorch/pull/143919))
- remove allow-untyped-defs from ao/quantization/experimental/fake_quantize.py ([#144091](https://github.com/pytorch/pytorch/pull/144091))
- [BE] typing for decorators ([#144161](https://github.com/pytorch/pytorch/pull/144161))
- remove allow-untyped-defs from ao/nn/sparse/quantized/utils.py ([#144232](https://github.com/pytorch/pytorch/pull/144232))
- [codemod] Remove unused-variable in caffe2/aten/src/ATen/native/quantized/cpu/fbgemm_utils.cpp +2 ([#144371](https://github.com/pytorch/pytorch/pull/144371))
- Migrate from Tuple -> tuple in torch/ao ([#144265](https://github.com/pytorch/pytorch/pull/144265))
- [BE][Easy] improve submodule discovery for `torch.ao` type annotations ([#144680](https://github.com/pytorch/pytorch/pull/144680))
- remove allow-untyped-defs from torch/ao/nn/quantized/reference/modules/linear.py ([#144656](https://github.com/pytorch/pytorch/pull/144656))
- remove allow-untyped-defs from torch/ao/nn/intrinsic/__init__.py ([#144652](https://github.com/pytorch/pytorch/pull/144652))
- PEP585 update - torch/_functorch ([#145139](https://github.com/pytorch/pytorch/pull/145139))
- PEP585 update - torch/ao/quantization ([#145140](https://github.com/pytorch/pytorch/pull/145140))
- PEP585 update - torch/ao ([#145199](https://github.com/pytorch/pytorch/pull/145199))
- PEP585 update - test ([#145176](https://github.com/pytorch/pytorch/pull/145176))
- Add TORCHINDUCTOR_VEC_ISA_OK env var for vec_isa_ok ([#134667](https://github.com/pytorch/pytorch/pull/134667))
- [Inductor][CPU] Add a lowering pass for _weight_int4pack_mm_for_cpu ([#145250](https://github.com/pytorch/pytorch/pull/145250))
- [BE]: Enable ruff rule SIM113 ([#147290](https://github.com/pytorch/pytorch/pull/147290))
- [Intel GPU] qlinear at XPU backend ([#133307](https://github.com/pytorch/pytorch/pull/133307))
- Fix typo ([#147330](https://github.com/pytorch/pytorch/pull/147330))
- fix pt2e block wise quantization unit test ([#147406](https://github.com/pytorch/pytorch/pull/147406))
- PEP585: More UP006 fixes ([#146392](https://github.com/pytorch/pytorch/pull/146392))
- [Intel GPU] qlinear_pointwise.binary[_tensor] XPU support ([#135337](https://github.com/pytorch/pytorch/pull/135337))
- [BE][Ez]: Use itertools.chain.from_iterable when possible ([#148190](https://github.com/pytorch/pytorch/pull/148190))
- Enable UBSAN test ([#147511](https://github.com/pytorch/pytorch/pull/147511))
- Use the device interface for detecting Triton availability ([#139171](https://github.com/pytorch/pytorch/pull/139171))
- Skip ao_sparsity TestComposability for missing FBGEMM ([#144146](https://github.com/pytorch/pytorch/pull/144146))
- [15/N] Fix extra warnings brought by clang-tidy-17 ([#143100](https://github.com/pytorch/pytorch/pull/143100))
- [5/N] Apply bugprone-unchecked-optional-access  ([#143111](https://github.com/pytorch/pytorch/pull/143111))
- Remove deprecated branch after capture_pre_autograd_graph fully migrate to training IR ([#143228](https://github.com/pytorch/pytorch/pull/143228), [#143426](https://github.com/pytorch/pytorch/pull/143426))
- [easy] Set feature use for aot autograd remote cache ([#143674](https://github.com/pytorch/pytorch/pull/143674))
- Fix cppcoreguidelines-pro-type-member-init ([#141787](https://github.com/pytorch/pytorch/pull/141787))
- change import relative paths due to internal build failures ([#143968](https://github.com/pytorch/pytorch/pull/143968))
- [4/N] Apply py39 ruff and pyupgrade fixes ([#143257](https://github.com/pytorch/pytorch/pull/143257))
- [torch][ao][EASY] Change print to log in numeric debugger to avoid large output ([#144790](https://github.com/pytorch/pytorch/pull/144790))
- [BE] typing for decorators - library ([#138969](https://github.com/pytorch/pytorch/pull/138969))
- [Accelerator] Use uniform `GetAllocator` for devices in `new_qtensor` function ([#144849](https://github.com/pytorch/pytorch/pull/144849))
- [2/N] Remove unnecessary once flag usage ([#145057](https://github.com/pytorch/pytorch/pull/145057))
- Fix cppcoreguidelines-init-variables ignorance ([#141795](https://github.com/pytorch/pytorch/pull/141795))
- Resolve affine quantization namespace collision with torchao ([#145941](https://github.com/pytorch/pytorch/pull/145941))
- Remove NOLINTNEXTLINE ([#146238](https://github.com/pytorch/pytorch/pull/146238))
- fix pt2e block wise quantization test ([#147035](https://github.com/pytorch/pytorch/pull/147035))
- Fix arvr macOS buck pytorch builds ([#147292](https://github.com/pytorch/pytorch/pull/147292))
- Intel XPU support for qconv.pointwise with mixed dtype ([#135465](https://github.com/pytorch/pytorch/pull/135465))

### security
