# UniAnimate-DiT 

An expanded version of [UniAnimate](https://arxiv.org/abs/2406.01188) based on [Wan2.1](https://github.com/Wan-Video/Wan2.1)

UniAnimate-DiT is based on a state-of-the-art DiT-based Wan2.1-14B-I2V model for consistent human image animation. This codebase is built upon [DiffSynth-Studio](https://github.com/modelscope/DiffSynth-Studio), thanks for the nice open-sourced project.

<div align="center">

<p align="center">
  <img src='https://github.com/user-attachments/assets/b7290f98-8b33-4626-945c-cf287ba84192' width='784'>

  Overview of the proposed UniAnimate-DiT
</p>

</div>


## 🔥 News 
- **[2025/04/21]** 🔥 We support Unified Sequence Parallel (USP) for multi-GPUs inference.
- **[2025/04/18]** 🔥🔥🔥 **We support teacache for both short video generation and long video generation, which can achieve about 4 times inference acceleration.** Now, it costs ~3 minutes to generate 5s 480p videos and ~13 minutes to generate 5s 720p videos on one A800 GPU. You can use teacache to select seed and disenable teacache for ideal results.
- **[2025/04/18]** 🔥 We support teacache, which can achieve about 4 times inference acceleration. It may have a slight impact on performance, and you can use teacache to select the seed. Long video generation does not currently support teacache acceleration, but we are working hard to overcome this.
- **[2025/04/16]** 🔥 The technical report is available on [ArXiv](https://arxiv.org/pdf/2504.11289).
- **[2025/04/15]** 🔥🔥🔥 We released the training and inference code of UniAnimate-DiT based on [UniAnimate](https://github.com/ali-vilab/UniAnimate) and [Wan2.1](https://github.com/Wan-Video/Wan2.1). The technical report will be available soon.


##  Demo cases
<table>
<center>
<tr>
    <!-- <td width=25% style="border: none"> -->
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/9671e4e1-edf4-4352-af1e-6743aff4e9f0" muted="false"></video>
    </td>
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/c3cf5dc6-19d2-4865-92b8-b687b4e7a901" muted="false"></video>
    </td>
</tr>
</table>



<table>
<center>
<tr>
    <!-- <td width=25% style="border: none"> -->
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/bd8a9dba-33b0-432f-8ae4-911d7044eb28" muted="false"></video>
    </td>
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/79601ec8-ed35-4542-9bb3-777085c6a4a0" muted="false"></video>
    </td>
</tr>
</table>


<table>
<center>
<tr>
    <!-- <td width=25% style="border: none"> -->
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/83ae10c3-9828-4eed-95db-f4e3265924b9" muted="false"></video>
    </td>
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/a6838591-4ed1-436e-b016-0c4d3864d92e" muted="false"></video>
    </td>
</tr>
</table>



<table>
<center>
<tr>
    <!-- <td width=25% style="border: none"> -->
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/9e2d75d3-8b1e-4cbb-91a5-dacf99c18261" muted="false"></video>
    </td>
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/32104e1a-4f20-4070-a458-73d9e9401013" muted="false"></video>
    </td>
</tr>
</table>



<table>
<center>
<tr>
    <!-- <td width=25% style="border: none"> -->
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/e7ae8deb-26e2-4452-844c-a8a043dd9846" muted="false"></video>
    </td>
    <td ><center>
        <video height="260" controls autoplay loop src="https://github.com/user-attachments/assets/7f96e347-617f-4c78-bc59-a2bcef9f8080" muted="false"></video>
    </td>
</tr>
</table>

## Getting Started with UniAnimate-DiT


### (1) Installation

Before using this model, please create the conda environment and install DiffSynth-Studio from **source code**.

```shell
conda create -n UniAnimate-DiT python=3.9.21
# or conda create -n UniAnimate-DiT python=3.10.16 # Python>=3.10 is required for Unified Sequence Parallel (USP)
conda activate UniAnimate-DiT

# CUDA 11.8
pip install torch==2.5.0 torchvision==0.20.0 torchaudio==2.5.0 --index-url https://download.pytorch.org/whl/cu118
# CUDA 12.1
pip install torch==2.5.0 torchvision==0.20.0 torchaudio==2.5.0 --index-url https://download.pytorch.org/whl/cu121
# CUDA 12.4
pip install torch==2.5.0 torchvision==0.20.0 torchaudio==2.5.0 --index-url https://download.pytorch.org/whl/cu124

git clone https://github.com/ali-vilab/UniAnimate-DiT.git
cd UniAnimate-DiT
pip install -e .
```

UniAnimate-DiT supports multiple Attention implementations. If you have installed any of the following Attention implementations, they will be enabled based on priority.

* [Flash Attention 3](https://github.com/Dao-AILab/flash-attention)
* [Flash Attention 2](https://github.com/Dao-AILab/flash-attention)
* [Sage Attention](https://github.com/thu-ml/SageAttention)
* [torch SDPA](https://pytorch.org/docs/stable/generated/torch.nn.functional.scaled_dot_product_attention.html) (default. `torch>=2.5.0` is recommended.)

## Inference


### (2) Download the pretrained checkpoints

(i) Download Wan2.1-14B-I2V-720P models using huggingface-cli:
```
pip install "huggingface_hub[cli]"
huggingface-cli download Wan-AI/Wan2.1-I2V-14B-720P --local-dir ./Wan2.1-I2V-14B-720P
```

Or download Wan2.1-14B-I2V-720P models using modelscope-cli:
```
pip install modelscope
modelscope download Wan-AI/Wan2.1-I2V-14B-720P --local_dir ./Wan2.1-I2V-14B-720P
```


(ii) Download pretrained UniAnimate-DiT models (only include the weights of lora and additional learnable modules):
```
pip install modelscope
modelscope download xiaolaowx/UniAnimate-DiT --local_dir ./checkpoints
```

Or download UniAnimate-DiT models using huggingface-cli:
```
pip install "huggingface_hub[cli]"
huggingface-cli download ZheWang123/UniAnimate-DiT --local-dir ./checkpoints
```

(iii) Finally, the model weights will be organized in `./checkpoints/` as follows:
```
./checkpoints/
|---- dw-ll_ucoco_384.onnx
|---- UniAnimate-Wan2.1-14B-Lora-12000.ckpt
└---- yolox_l.onnx
```



### (3) Pose alignment 

Rescale the target pose sequence to match the pose of the reference image (you can also install `pip install onnxruntime-gpu==1.18.1` for faster extraction on GPU.):
```
# reference image 1
python run_align_pose.py  --ref_name data/images/WOMEN-Blouses_Shirts-id_00004955-01_4_full.jpg --source_video_paths data/videos/source_video.mp4 --saved_pose_dir data/saved_pose/WOMEN-Blouses_Shirts-id_00004955-01_4_full 

# reference image 2
python run_align_pose.py  --ref_name data/images/musk.jpg --source_video_paths data/videos/source_video.mp4 --saved_pose_dir data/saved_pose/musk 

# reference image 3
python run_align_pose.py  --ref_name data/images/WOMEN-Blouses_Shirts-id_00005125-03_4_full.jpg --source_video_paths data/videos/source_video.mp4 --saved_pose_dir data/saved_pose/WOMEN-Blouses_Shirts-id_00005125-03_4_full

# reference image 4
python run_align_pose.py  --ref_name data/images/IMG_20240514_104337.jpg --source_video_paths data/videos/source_video.mp4 --saved_pose_dir data/saved_pose/IMG_20240514_104337

# reference image 5
python run_align_pose.py  --ref_name data/images/10.jpg --source_video_paths data/videos/source_video.mp4 --saved_pose_dir data/saved_pose/10

# reference image 6
python run_align_pose.py  --ref_name data/images/taiyi2.jpg --source_video_paths data/videos/source_video.mp4 --saved_pose_dir data/saved_pose/taiyi2
```
The processed target pose for demo videos will be in ```data/saved_pose```. `--ref_name` denotes the path of reference image, `--source_video_paths` provides the source poses, `--saved_pose_dir` means the path of processed target poses.


### (4) Run UniAnimate-DiT-14B to generate 480P videos

```
CUDA_VISIBLE_DEVICES="0" python examples/unianimate_wan/inference_unianimate_wan_480p.py
```
About 23G GPU memory is needed. After this, 81-frame video clips with 832x480 (hight x width) resolution will be generated under the `./outputs` folder.

- **Tips**: you can also set `cfg_scale=1.0` to save inference time, which disables classifier-free guidance and can double the speed with minimal performance impact. https://github.com/ali-vilab/UniAnimate-DiT/blob/c2c7019dbb081464271d470d750b7693ade10dd8/examples/unianimate_wan/inference_unianimate_wan_480p.py#L223-L224

- **Tips**: you can set `num_persistent_param_in_dit` to a small number to reduce VRAM required.

|`torch_dtype`|`num_persistent_param_in_dit`|Speed|Required VRAM|Default Setting|
|-|-|-|-|-|
|torch.bfloat16|7*10**9 (7B)|20.5s/it|23G|yes|
|torch.bfloat16|0|23.0s/it|14G||

- **Tips**: you can set `use_teacache=True` to enable teacache, which can achieve about 4 times inference acceleration. It may have a slight impact on performance, and you can also use teacache to select the seed. 

If you have many GPUs for inference, we also support Unified Sequence Parallel (USP), note that python>=3.10 is required for Unified Sequence Parallel (USP):

```
pip install xfuser
torchrun --standalone --nproc_per_node=4 examples/unianimate_wan/inference_unianimate_wan_480p_usp.py
```

For long video generation, run the following comment, the tips above can also be used by yourself:

```
CUDA_VISIBLE_DEVICES="0" python examples/unianimate_wan/inference_unianimate_wan_long_video_480p.py
```

### (5) Run UniAnimate-DiT-14B to generate 720P videos

```
CUDA_VISIBLE_DEVICES="0" python examples/unianimate_wan/inference_unianimate_wan_720p.py
```
About 36G GPU memory is needed. After this, 81-frame video clips with 1280x720 resolution will be generated.

- **Tips**: you can also set `cfg_scale=1.0` to save inference time, which disables classifier-free guidance and can double the speed with minimal performance impact. https://github.com/ali-vilab/UniAnimate-DiT/blob/c37c996740cb9584edbdf3b4db2fa9eb47526e30/examples/unianimate_wan/inference_unianimate_wan_720p.py#L224-L225

- **Tips**: you can set `num_persistent_param_in_dit` to a small number to reduce VRAM required.

|`torch_dtype`|`num_persistent_param_in_dit`|Speed|Required VRAM|Default Setting|
|-|-|-|-|-|
|torch.bfloat16|7*10**9 (7B)|20.5s/it|36G|yes|
|torch.bfloat16|0|23.0s/it|26G||

- **Tips**: you can set `use_teacache=True` to enable teacache, which can achieve about 4 times inference acceleration. It may have a slight impact on performance, and you can also use teacache to select the seed. 


**Note**: Even though our model was trained on 832x480 resolution, we observed that direct inference on 1280x720 is usually allowed and produces satisfactory results. 


For long video generation, run the following comment, the tips above can also be used by yourself:

```
CUDA_VISIBLE_DEVICES="0" python examples/unianimate_wan/inference_unianimate_wan_long_video_720p.py
```

**Note**: We find use teacache for 720P long video generation may lead to inconsistent background. We still work on it. You can use teacache to select random seed and disenable teacache for ideal results.

## Train

We support UniAnimate-DiT training on our own dataset. 

### Step 1: Install additional packages

```
pip install peft lightning pandas
# deepspeed for multiple GPUs
pip install -U deepspeed
```

### Step 2: Prepare your dataset

In order to speed up the training, we preprocessed the videos, extracted video frames and corresponding Dwpose in advance, and packaged them with pickle package. You need to manage the training data as follows:

```
data/example_dataset/
└── TikTok
    └── 00001_mp4
      ├── dw_pose_with_foot_wo_face.pkl # packaged Dwpose
      └── frame_data.pkl # packaged frames
```

We encourage adding large amounts of data to finetune models to get better results. The experimental results show that about 1000 training videos can finetune a good human image animation model. Please refer to `prepare_training_data.py` file for more details about packaged Dwpose/frames.

### Step 3: Train

For convenience, we do not pre-process VAE features, but put VAE pre-processing and DiT model training in a training script, and also facilitate data augmentation to improve performance. You can also choose to extract VAE features first and then conduct subsequent DiT model training. 


LoRA training (One A100 GPU):

```shell
CUDA_VISIBLE_DEVICES="0" python examples/unianimate_wan/train_unianimate_wan.py \
   --task train  \
   --train_architecture lora \
   --lora_rank 64 --lora_alpha 64  \
   --dataset_path data/example_dataset   \
   --output_path ./models_out_one_GPU   \
   --dit_path "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00001-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00002-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00003-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00004-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00005-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00006-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00007-of-00007.safetensors"    \
   --max_epochs 10   --learning_rate 1e-4   \
   --accumulate_grad_batches 1   \
   --use_gradient_checkpointing --image_encoder_path "./Wan2.1-I2V-14B-720P/models_clip_open-clip-xlm-roberta-large-vit-huge-14.pth"  --use_gradient_checkpointing_offload 
```


LoRA training (Multi-GPUs, based on `Deepseed`):

```shell
CUDA_VISIBLE_DEVICES="0,1,2,3" python examples/unianimate_wan/train_unianimate_wan.py  \
   --task train   --train_architecture lora \
   --lora_rank 128 --lora_alpha 128  \
   --dataset_path data/example_dataset   \
   --output_path ./models_out   --dit_path "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00001-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00002-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00003-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00004-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00005-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00006-of-00007.safetensors,./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00007-of-00007.safetensors"     \
   --max_epochs 10   --learning_rate 1e-4   \
   --accumulate_grad_batches 1   \
   --use_gradient_checkpointing \
   --image_encoder_path "./Wan2.1-I2V-14B-720P/models_clip_open-clip-xlm-roberta-large-vit-huge-14.pth" \
   --use_gradient_checkpointing_offload \
   --training_strategy "deepspeed_stage_2" 
```


You can also finetune our trained model by set `--pretrained_lora_path="./checkpoints/UniAnimate-Wan2.1-14B-Lora-12000.ckpt"`.

### Step 4: Test

Test the LoRA finetuned model trained on one GPU:

```python
import torch
from diffsynth import ModelManager, WanVideoPipeline, save_video, VideoData, WanUniAnimateVideoPipeline


# Load models
model_manager = ModelManager(device="cpu")
model_manager.load_models(
    ["./Wan2.1-I2V-14B-720P/models_clip_open-clip-xlm-roberta-large-vit-huge-14.pth"],
    torch_dtype=torch.float32, # Image Encoder is loaded with float32
)
model_manager.load_models(
    [
        [
            
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00001-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00002-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00003-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00004-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00005-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00006-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00007-of-00007.safetensors",

        ],
        "./Wan2.1-I2V-14B-720P/models_t5_umt5-xxl-enc-bf16.pth",
        "./Wan2.1-I2V-14B-720P/Wan2.1_VAE.pth",
    ],
    torch_dtype=torch.bfloat16, 
)

model_manager.load_lora_v2("models/lightning_logs/version_1/checkpoints/epoch=0-step=500.ckpt", lora_alpha=1.0)

...
...
```

Test the LoRA finetuned model trained on multi-GPUs based on Deepspeed, first you need `python zero_to_fp32.py . output_dir/ --safe_serialization` to change the .pt files to .safetensors files. Note that `zero_to_fp32.py` is an automatically generated file that can be found in the checkpoint folder after training with DeepSpeed on ​​Multi-GPUs. And then run:

```python
import torch
from diffsynth import ModelManager, WanVideoPipeline, save_video, VideoData, WanUniAnimateVideoPipeline


# Load models
model_manager = ModelManager(device="cpu")
model_manager.load_models(
    ["./Wan2.1-I2V-14B-720P/models_clip_open-clip-xlm-roberta-large-vit-huge-14.pth"],
    torch_dtype=torch.float32, # Image Encoder is loaded with float32
)
model_manager.load_models(
    [
        [
            
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00001-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00002-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00003-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00004-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00005-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00006-of-00007.safetensors",
            "./Wan2.1-I2V-14B-720P/diffusion_pytorch_model-00007-of-00007.safetensors",

        ],
        "./Wan2.1-I2V-14B-720P/models_t5_umt5-xxl-enc-bf16.pth",
        "./Wan2.1-I2V-14B-720P/Wan2.1_VAE.pth",
    ],
    torch_dtype=torch.bfloat16, 
)

model_manager.load_lora_v2([
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00001-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00002-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00003-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00004-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00005-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00006-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00007-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00008-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00009-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00010-of-00011.safetensors",
            "./models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt/output_dir/model-00011-of-00011.safetensors",
            ], lora_alpha=1.0)

...
...
```


## Citation

If you find this codebase useful for your research, please cite the following paper:

```
@article{wang2025unianimate,
      title={UniAnimate: Taming Unified Video Diffusion Models for Consistent Human Image Animation},
      author={Wang, Xiang and Zhang, Shiwei and Gao, Changxin and Wang, Jiayu and Zhou, Xiaoqiang and Zhang, Yingya and Yan, Luxin and Sang, Nong},
      journal={Science China Information Sciences},
      year={2025}
}
```


## Disclaimer

This project is intended for academic research, and we explicitly disclaim any responsibility for user-generated content. Users are solely liable for their actions while using the generative model. The project contributors have no legal affiliation with, nor accountability for, users' behaviors. It is imperative to use the generative model responsibly, adhering to both ethical and legal standards.
