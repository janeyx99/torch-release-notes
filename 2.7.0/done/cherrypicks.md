# cherry picks
## bc breaking


## deprecation


## new features


## improvements
### Profiler
- Add HPU availabilities to profiler (#149115)
### Inductor
- Add AOTI shim for `_weight_int4pack_mm_cpu_tensor` (#149031)
- Remove runtime dependency on packaging (#149125) 


## bug fixes
### Inductor
- Fix backwards compatibility for `AOTIModelPackageLoader()` constructor defaults (#149082)
- Fix blank space break windows file path (#149388)
- Fix inductor windows linker error (#150256)
### Build Frontend
- Fix atomic operation compatibility for ARMv8-A (Raspberry Pi 4) by adjusting compilation flags (#148070)
- Make PyTorch buildable by CMake-4.x (#150203)
### torch.compile
- Do not depend on numpy during `torch._functorch` import (#149683)
### torch.export
- Symintify `transpose_` (#149057)
### MPSInductor
- Move threadfence to before first read from shared memory, not after (#149437)
### MPS
- Fix attention `enable_gqa` crash on MPS (#149147)
- Fix dot/mm for conj_tensors (#150157)
- Fix `tril` op not handling infs correctly (#149866)
### ROCm
- Fixes and improvements to CUDA->HIP flag conversion for CPP extensions (#149245)
### CUDA
- Fix path lookup in `_preload_cuda_deps` (#149808)
- Help support Blackwell: Fix backward launch bounds again for `sm100`, `sm120` (#150640)
### XPU
- Enabling XPU in `OffsetBasedRNGTracker` to unbreak `torch.distributed` (#148360)
- `torch.backends.mkldnn.flags()` CM should not warn (#150358)


## performance
### Intel
- use zero-point to decide `conv` src zp mask (#149473)
### Inductor 
- Don't exclude `constant_pad_nd` in prologue fusion (#149947) - actually, reverted.
### ROCm
- change preferred blas lib defaults (#150212)

## docs
### ONNX
- Expose verification utilities (#148603)
### Build Frontend
- Removing doc references to PRE_CXX11_ABI. (#149756)

## devs
### XPU
- Fix XPU builds inside venv (#150300)

## Untopiced
## security


## not user facing
- [inductor] Fix profiler tests with latest Triton (#149059) 
- [RELEASE ONLY CHANGES] Apply release only changes to release 2.7 (#149056)
- [cherry-pick] Revert #148823 - Make dynamism code robust to NotImplementedException (#149160)
- Add release branch push triggers to rocm-mi300.yml (#149517)
- Pin auditwheel to 6.2.0 (#149471)
- Add release branch push triggers to inductor-rocm-mi300.yml (#149672)
- Update ExecuTorch pin update (#149539)
- Revert "[CI] Don't clean workspace when fetching repo (#147994)" (#149129)
- Automate stable CUDA update and linter using min Python verison (#148912)
- nccl: upgrade to 2.26.2 to avoid hang on ncclCommAbort (#149351)
- Modify cuda aarch64 install for cudnn and nccl. Cleanup aarch64 cuda 12.6 docker (#149540)
- ci/docker: use NCCL 2.26.2-1 (#149778)
- Parallelize sort (#149505), Revert "Parallelize sort (#149765)" -- cancels out
- update release 2.7 xla pin (#150126)
- [Release-only] Pin intel-oneapi-dnnl to 2025.0.1-6 (#150132)
- Pin cmake==3.31.6 (#150193)
- Pin cmake to 3.31.2 for windows conda install (#150185)
- Revert "[PGNCCL] Launch kernel on current stream & remove `record_stream` entirely (#148590) (#150352)
- [ROCm] cmake 4 workaround for hiprtc (#150324)
- [CI] Disable some tests that are failing in periodic (#150059)
- [BE] Fix triton windows build (#150512)
- Revert "[fx] Move Node._prepend/Node._remove_from_list to C++ (#148261)" (#150572)
- [Release/2.7][MPS] Warn that torch.compile is a prototype (#150550)
- Update expected results for pr_time_benchmarks (#150620)
- Revert "[ROCm] change preferred blas lib defaults (#150249)" (#150658)
- Reland of "[ROCm] change preferred blas lib defaults (#150249)" (#150707)


## Added to final.md directly
- [regression] Fix pin_memory() when it is called before device lazy initialization. #149033 -- added to be with 145752
- [AOTI][XPU] Fix: model_container_runner_xpu.cpp is not built into libtorch_xpu.so #149175 -- already in final.md
- op should NOT be static in aoti_torch_call_dispatcher (#149208) -- added in final.md with "Support libtorch-agnostic extensions with stable torch ABI"
- Use schema as source of truth + support ones_like/empty_like (#149052)
- Update triton commit to fix to fix level_zero not found by env var LEVEL_ZERO_V1_SDK_PATH. (#149511)
- Add triton as dependency to CUDA aarch64 build (#149584)
- Update AOTInductor documentation for XPU support (#149299)
- [inductor][triton 3.3] Fix cpp_wrapper w/ TMA in triton 3.3 (#149973)
- Update Doc for Intel XPU Profiling (#134515)
- [Doc] Update CMAKE_PREFIX_PATH for XPU windows README (#148863)
- update get start xpu document for v2.7 (#150397)
