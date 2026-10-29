# 生图工作台 · 模型与依赖

本页文件大小采用十进制 GB（1 GB = 10⁹ 字节），来自已下载文件或上游 Hugging Face 文件元数据；下载体积不等于显存占用。作者设备为 NVIDIA RTX 5060 Ti 16GB、Intel Core i7-12700KF、64GB 内存，Windows 11。其他设备的速度与最大尺寸取决于驱动、PyTorch/CUDA、分辨率、步数与内存卸载。无需一次下载所有可选路线。

ComfyUI 应使用支持这些模型的版本；作者现有核心为 0.43.0、前端 1.57.4。安装时先核对 `/object_info` 中所需节点，不能只凭版本号判断。RTX 50 系需要支持 Blackwell 的 NVIDIA 驱动及 PyTorch CUDA 构建；先执行 `python -c "import torch; print(torch.__version__,torch.cuda.is_available(),torch.cuda.get_device_name(0))"` 核验。保留现有可工作的 PyTorch，切勿盲目降级。

## 按路线下载

| 路线 | 文件与大小 | ComfyUI 下的放置位置 | 官方或作者来源 |
|---|---|---|---|
| Qwen Image 2.1 基础 INT8 | `qwen_image_2.1_int8_convrot.safetensors` · 7.257 GB | `models/diffusion_models/` | [Comfy-Org/Qwen-Image-2.1](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/tree/main/diffusion_models) |
| Qwen Image 2.1 Turbo INT8 | `qwen_image_2.1_turbo_int8_convrot.safetensors` · 7.257 GB | `models/diffusion_models/` | [同上](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/tree/main/diffusion_models) |
| 可选 UC GGUF | `qwen-image-2.1-UC-Q6_K.gguf` · 5.877 GB | `models/diffusion_models/` | [KasugaiSakura 原文件](https://huggingface.co/KasugaiSakura/Qwen-Image-2.1-Uncensored-Abenzerps-GGUF/blob/main/qwen-image-2.1-UC-Q6_K.gguf) |
| Qwen 编码器 | `qwen3vl_8b_int8_convrot.safetensors` · 9.351 GB | `models/text_encoders/` | [Comfy-Org/Qwen-Image-2.1](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/tree/main/text_encoders) |
| Qwen 2.1 VAE | `qwen_image_2.1_vae_bf16.safetensors` · 0.676 GB | `models/vae/` | [Comfy-Org/Qwen-Image-2.1](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/tree/main/vae) |
| MOSS 官方 SFT | `moss_image_sft_bf16.safetensors` · 10.165 GB | `models/diffusion_models/` | [Comfy-Org/MOSS-Image](https://huggingface.co/Comfy-Org/MOSS-Image/tree/main/diffusion_models) |
| MOSS Jib V2 写实 | `jibMixMossRealistic_v2RealityRenew.safetensors` · 5.414 GB | `models/diffusion_models/` | [Jib Mix MOSS Realistic V2](https://civitai.com/models/3016339/jib-mix-moss-realistic?modelVersionId=3435334) |
| MOSS 编码器 | `qwen3.5_9b_bf16.safetensors` · 19.306 GB | `models/text_encoders/` | [Comfy-Org/MOSS-Image](https://huggingface.co/Comfy-Org/MOSS-Image/tree/main/text_encoders) |
| MOSS VAE | `flux2-vae_small.safetensors` · 0.250 GB | `models/vae/` | [Comfy-Org/MOSS-Image](https://huggingface.co/Comfy-Org/MOSS-Image/tree/main/vae) |
| Krea 2 Turbo | `krea2_turbo_fp8_scaled.safetensors` · 13.142 GB | `models/diffusion_models/` | [Comfy-Org/Krea-2](https://huggingface.co/Comfy-Org/Krea-2/tree/main/diffusion_models) |
| Krea 编码器 | `qwen3vl_4b_fp8_scaled.safetensors` · 5.242 GB | `models/text_encoders/` | [Comfy-Org/Krea-2](https://huggingface.co/Comfy-Org/Krea-2/tree/main/text_encoders) |
| Krea VAE | `qwen_image_vae.safetensors` · 0.254 GB | `models/vae/` | [Comfy-Org/Krea-2](https://huggingface.co/Comfy-Org/Krea-2/tree/main/vae) |
| Z-Image Turbo | `z_image_turbo_bf16.safetensors` · 12.310 GB | `models/diffusion_models/` | [Comfy-Org/z_image_turbo](https://huggingface.co/Comfy-Org/z_image_turbo/tree/main/split_files) |
| Z-Image 编码器 | `qwen_3_4b.safetensors` · 8.045 GB | `models/text_encoders/` | [同上](https://huggingface.co/Comfy-Org/z_image_turbo/tree/main/split_files/text_encoders) |
| Z-Image VAE | `ae.safetensors` · 0.335 GB | `models/vae/` | [同上](https://huggingface.co/Comfy-Org/z_image_turbo/tree/main/split_files/vae) |

MOSS 原生是文生图模型；本工作台的参考图重绘通过 VAE 编码和降噪实现，不等同于原生多图语义编辑。Qwen 与 BBAPI 承担模型实际支持的多图编辑。Krea 2 Turbo 的参考能力以当前节点实现与模型卡为准；不得把重绘描述为原生指令编辑。三套 VAE/编码器不能混用。

## 共用可选模型

| 功能 | 文件 | 大小 | 位置与来源 |
|---|---|---:|---|
| SeedVR2 超分 | `seedvr2_ema_3b_fp8_e4m3fn.safetensors` | 3.392 GB | `models/seedvr2/` · [numz/SeedVR2_comfyUI](https://huggingface.co/numz/SeedVR2_comfyUI) |
| SeedVR2 VAE | `ema_vae_fp16.safetensors` | 0.501 GB | `models/seedvr2/` · 同上。保持这个上游文件名，勿改成带 `seedvr2_` 的名字。|
| CPU 语音识别 | `model.onnx` | 0.940 GB | `models/sensevoice-small/` · [kautism/SenseVoiceSmall-onnx](https://huggingface.co/kautism/SenseVoiceSmall-onnx) |

SenseVoice 还需同仓库 `config.yaml`、`tokens.json`、`am.mvn`；整套文件置于同一文件夹。依赖 `onnxruntime`、`kaldi-native-fbank`、`PyYAML`、`numpy`，仅使用 CPU。可用环境变量 `BBAIGC_SENSEVOICE_MODEL_DIR` 指定已有模型目录；兼容旧变量 `CODEX_SENSEVOICE_MODEL_DIR`，两者同时设置时优先采用 BBAIGC 变量。

提示词优化使用本机 [Ollama](https://ollama.com/download)。公开可部署的起点是 [qwen3.5:9b](https://ollama.com/library/qwen3.5)，执行 `ollama pull qwen3.5:9b`，在节点中选实际安装名称。作者的 `uncensored-*` 是自定义本机模型别名，不能假定其他用户可以直接拉取。调用地址优先 `http://127.0.0.1:11434`。

## 节点来源

- 共享工作台插件：[BBAIGC/ComfyUI-BBAIGC-MiniMax-H3](https://github.com/BBAIGC/ComfyUI-BBAIGC-MiniMax-H3)。只安装一份；图片与视频工作台共用。
- BBAPI 路线：[BBAIGC/ComfyUI-BBAIGC-BBAPI](https://github.com/BBAIGC/ComfyUI-BBAIGC-BBAPI)。密钥仅在用户自己的本机配置，不进入 JSON。
- GGUF 加载：[city96/ComfyUI-GGUF](https://github.com/city96/ComfyUI-GGUF)。仅 GGUF 路线需要。
- SeedVR2：[numz/ComfyUI-SeedVR2_VideoUpscaler](https://github.com/numz/ComfyUI-SeedVR2_VideoUpscaler)。
- VOSR2：[ylchen333/ComfyUI-VOSR2](https://github.com/ylchen333/ComfyUI-VOSR2)。
- DLSS5：[RH-RunningHub/ComfyUI-RH-DLSS5](https://github.com/RH-RunningHub/ComfyUI-RH-DLSS5)。按其 NVIDIA GPU 与驱动要求安装；不支持时选其他超分路线。

不使用的可选插件不必安装；保持对应功能关闭。模型下载可能需要接受上游许可或登录，相应许可由各模型作者管理。

## 命令下载示例

在 ComfyUI 自己的 Python 环境安装 `huggingface_hub`，再使用 `hf download`：

```powershell
$ComfyRoot = '你的ComfyUI路径'
hf download Comfy-Org/Qwen-Image-2.1 diffusion_models/qwen_image_2.1_turbo_int8_convrot.safetensors text_encoders/qwen3vl_8b_int8_convrot.safetensors vae/qwen_image_2.1_vae_bf16.safetensors --local-dir "$ComfyRoot/models"
hf download kautism/SenseVoiceSmall-onnx model.onnx config.yaml tokens.json am.mvn --local-dir "$ComfyRoot/models/sensevoice-small"
```

MOSS 国内镜像：[ModelScope Comfy-Org/MOSS-Image](https://modelscope.cn/models/Comfy-Org/MOSS-Image)，保持上表精确文件名。不要只下载索引文件，也不要把其他量化文件改名冒充表中的模型。

## 选择作者同款或适配显存的量化版本

先选择：**作者同款**、**适配本机显存**、**高配更高精度**，或同时保留同款与一个适配版本。安装智能体先检查硬件、现有模型和磁盘，再一次性确认尚未指定的方案；不要默认下载全部档位。

作者图片主模型是社区 UC（Uncensored）Q6_K，配套 Qwen 编码器 INT8 与 2.1 VAE。UC 是社区权重变体名称，与本工作台功能无关；原模型和变体各自的许可仍适用。下表显存档位是选择起点，**不是最低显存保证或逐档实测**；分辨率、批量、编码器精度和 CPU/内存卸载都会改变占用，64GB 内存也不能替代足够磁盘。

| UC GGUF 主模型 | 文件大小 | 可优先评估的设备与取舍 |
|---|---:|---|
| `qwen-image-2.1-UC-Q4_0.gguf` | 4.152 GB | 8–12GB 显存先评估；体积最小，量化损失可能更明显，需卸载与小尺寸验证 |
| `qwen-image-2.1-UC-Q4_K_M.gguf` | 4.605 GB | 8–12GB 显存的另一低体积起点；仍需配套编码器与卸载 |
| `qwen-image-2.1-UC-Q5_K_M.gguf` | 5.221 GB | 12–16GB 显存可评估的体积/精度折中 |
| `qwen-image-2.1-UC-Q6_K.gguf` | 5.877 GB | 作者 5060 Ti 16GB 同款；保留已有已验证配置的优先选择 |
| `qwen-image-2.1-UC-Q8_0.gguf` | 7.592 GB | 24GB 及以上可评估；更大不代表固定更快 |
| `qwen-image-2.1-UC-BF16.gguf` | 14.230 GB | 高显存/充足内存设备可评估；近完整精度，磁盘与内存代价最高 |

以上精确文件来自 [KasugaiSakura UC GGUF 仓库](https://huggingface.co/KasugaiSakura/Qwen-Image-2.1-Uncensored-Abenzerps-GGUF/tree/main)，放入 `models/diffusion_models/`，用 GGUF 加载节点选择实际文件名。不要把 MLX 文件用于 ComfyUI 的 PyTorch/GGUF 路线；该仓库的 NVFP4/FP8 等是不同格式，不能仅修改扩展名替代 GGUF。

Qwen 2.1 官方编码器还有 [W4A8 6.312 GB、INT8 9.351 GB、BF16 17.534 GB](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/tree/main/text_encoders)。主模型量化与编码器量化是两个选择；作者用 INT8，低显存可在当前核心确认兼容后评估 W4A8，高配可评估 BF16。统一放入 `models/text_encoders/`，不要误用 PE 专用编码器代替本路线的 `qwen3vl_8b_*`。

| 其他模型的同架构档位 | 大小 | 选择建议 |
|---|---:|---|
| Qwen 2.1 官方基础/ Turbo INT8 ConvRot | 各 7.257 GB | 当前公开 Turbo 默认；配套编码器/VAE 不变 |
| Qwen 2.1 官方基础/ Turbo BF16 | 各 14.230 GB | 高显存设备可评估；不能把基础模型和 Turbo 采样参数混用 |
| Krea 2 Turbo NVFP4 | 7.674 GB | 对应 GPU 与当前 ComfyUI 必须支持该格式；RTX 50 系可作为待验证选择，未逐档实测 |
| Krea 2 Turbo FP8 | 13.142 GB | 本工作台公开默认，较 BF16 节省体积 |
| Krea 2 Turbo BF16 | 26.283 GB | 高配更高精度选择；非低显存默认 |

Krea 的编码器对应 [FP8 5.242 GB、INT8 ConvRot 4.859 GB、BF16 8.876 GB](https://huggingface.co/Comfy-Org/Krea-2/tree/main/text_encoders)，VAE 为同仓库 `qwen_image_vae.safetensors`。默认 FP8 配套组合已写在前面的模型表；更换量化后按真实文件名选择，不能与 Qwen 2.1 的 VAE/编码器混用。

这些变体文件与大小于 2026-10-30 从上游文件元数据核对；不同档位的生成质量、峰值显存和速度仍需安装者在自己的配置下验证。
