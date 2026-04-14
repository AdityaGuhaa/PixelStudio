# PixelStudio

## Overview

PixelStudio is a locally deployed AI-powered image generation system built using modern diffusion models. The project focuses on enabling high-quality image synthesis using GPU acceleration, while maintaining a clean, reproducible, and scalable development environment.

This project was built as a hands-on exploration into:

* Diffusion models (Stable Diffusion / SDXL)
* Local AI inference pipelines
* GPU-based ML environments
* Dependency management and system design

## Pipeline

<img width="1918" height="1078" alt="github" src="https://github.com/user-attachments/assets/d5fc4e3c-f219-4bd5-9503-7421b068164d" />

## Result

<img width="768" height="768" alt="ComfyUI_00001_" src="https://github.com/user-attachments/assets/9674515c-a7c6-42e1-b80a-14f37fc70b41" />

## Background Activity
<img width="1918" height="1078" alt="backend" src="https://github.com/user-attachments/assets/ae1a6e05-1a2b-4880-b116-66b3c1cf9875" />

## Objective

The primary objectives of PixelStudio were:

* Build a local image generation system without cloud dependency
* Understand diffusion model pipelines at a systems level
* Learn real-world ML environment setup and debugging
* Create a modular and extensible AI experimentation platform

---

## Project Architecture

```
PixelStudio/
│
├── ComfyUI/
│   ├── models/
│   │   ├── checkpoints/
│   │   ├── loras/
│   │   └── controlnet/
│
├── outputs/
├── workflows/
├── notes/
│
├── requirements.txt
├── requirements.lock.txt
```

---

## Tech Stack

* Frontend/UI: ComfyUI (node-based interface)
* Backend: Python
* ML Framework: PyTorch (CUDA enabled)
* Model: Juggernaut XL (SDXL-based checkpoint)
* Environment: Conda (Python 3.10)
* Hardware: NVIDIA RTX 4050 (6GB VRAM)

---

## Complete Execution Process

### Step 1: Environment Setup

* Created isolated conda environment

```
conda create -n pixelstudio python=3.10
conda activate pixelstudio
```

* Installed PyTorch manually using pre-downloaded CUDA 12.1 wheels

---

### Step 2: Dependency Resolution

Problems faced:

* NumPy 2.x incompatibility with PyTorch
* OpenCV dependency conflicts
* Missing ComfyUI dependencies (sqlalchemy, alembic)

Solutions:

```
pip install "numpy<2"
pip install opencv-python==4.8.1.78
```

* Installed missing dependencies manually
* Stabilized environment using tested versions

---

### Step 3: PyTorch Version Debugging

Problem:

* Older PyTorch versions lacked required APIs (torch.library.custom_op)

Attempts:

* 2.2.2 → failed
* 2.3.1 → still failed

Final solution:

* Installed PyTorch 2.4.1 (CUDA 12.1 compatible)

---

### Step 4: ComfyUI Setup

* Cloned repository manually into project folder
* Resolved directory conflicts
* Installed dependencies using requirements file

Issues:

* Missing module: comfy_kitchen

Resolution:

* Continued with fallback (non-critical for basic generation)

---

### Step 5: Model Selection and Setup

Problem:

* HuggingFace models mostly in diffusers format

Solution:

* Switched to safetensors checkpoint format
* Downloaded JuggernautXL (6.6GB)

Placed at:

```
ComfyUI/models/checkpoints/
```

---

## Diffusion Pipeline (Actual Implementation)

The system uses a node-based pipeline in ComfyUI.

### Nodes Used

* Load Checkpoint
* CLIP Text Encode (Positive)
* CLIP Text Encode (Negative)
* Empty Latent Image
* KSampler
* VAE Decode
* Save Image

---

## Pipeline Flow (Step-by-Step)

### 1. Model Loading

Load Checkpoint provides:

* MODEL → KSampler
* CLIP → both text encoders
* VAE → VAE Decode

---

### 2. Prompt Encoding

Two CLIP encoders are used:

* Positive prompt → guides generation
* Negative prompt → suppresses unwanted features

Connections:

* Positive CLIP → KSampler (positive)
* Negative CLIP → KSampler (negative)

---

### 3. Latent Space Initialization

* Empty Latent Image defines:

  * resolution: 768x768
  * batch size: 1

Connection:

* Latent → KSampler (latent_image)

---

### 4. Diffusion Sampling

KSampler performs the denoising process

Configuration used:

* Steps: 20–25
* CFG: 7–8
* Sampler: euler (initial) → dpmpp_2m (optimized)
* Scheduler: simple → normal

---

### 5. Image Decoding

* KSampler output (latent) → VAE Decode
* VAE converts latent to image space

---

### 6. Output Saving

* VAE Decode → Save Image
* Images stored in outputs folder

---

## Final Pipeline Representation

```
Text Prompt → CLIP Encoding → Conditioning → KSampler → Latent → VAE Decode → Image
```

---

## Prompts Used

### Initial Prompt

Positive:

```
ultra realistic portrait, cinematic lighting, sharp focus, 85mm lens, detailed skin texture
```

Negative:

```
blurry, low quality, distorted face, bad anatomy, extra fingers
```

---

### Improved Prompt

Positive:

```
ultra realistic portrait, natural skin texture, soft cinematic lighting, 85mm lens, realistic pores, balanced exposure
```

Negative:

```
plastic skin, oversharpened, overexposed, unrealistic texture, blurry, bad anatomy
```

---

## Key Problems Faced

### 1. Slow Downloads

* Pip downloads were slow
* Used manual wheel downloads

---

### 2. Dependency Conflicts

* NumPy vs PyTorch mismatch
* OpenCV version incompatibility

---

### 3. PyTorch Compatibility

* Missing APIs in older versions

---

### 4. ComfyUI Dependency Issues

* Missing alembic and database modules

---

### 5. Model Format Confusion

* Diffusers vs safetensors mismatch

---

### 6. Pipeline Wiring Confusion

* Initial incorrect connections between positive and negative prompts

Resolution:

* Corrected mapping:

  * Positive → KSampler positive
  * Negative → KSampler negative

---

## Key Learnings

* Diffusion systems are pipeline-based, not single-step models
* Correct wiring is critical for control
* ML environments are highly version-sensitive
* GPU optimization is essential for performance
* Negative prompts play a major role in output quality

---

## How to Run

```
conda activate pixelstudio
cd ComfyUI
python main.py
```

Open:

```
http://127.0.0.1:8188
```

---

## Output

* Resolution: 768x768
* Generation time: ~10–30 seconds (RTX 4050)
* Output directory: ComfyUI/output/

---

## Future Scope

* LoRA integration for styles
* ControlNet pipelines
* Workflow automation
* Backend API integration
* Multi-model support

---

## Conclusion

PixelStudio demonstrates a complete end-to-end local AI image generation pipeline, including environment setup, dependency debugging, model integration, and pipeline construction.

This project reflects practical understanding of diffusion systems, GPU inference, and real-world ML engineering challenges.

---

## Author

Aditya Guha
AI & Machine Learning Enthusiast
