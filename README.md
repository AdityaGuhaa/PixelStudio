# PixelStudio

## Overview

PixelStudio is a locally deployed AI-powered image generation system built using modern diffusion models. The project focuses on enabling high-quality image synthesis using GPU acceleration, while maintaining a clean, reproducible, and scalable development environment.

This project was built as a hands-on exploration into:

* Diffusion models (Stable Diffusion / SDXL)
* Local AI inference pipelines
* GPU-based ML environments
* Dependency management and system design

---

## Objective

The primary objectives of PixelStudio were:

* Build a **local image generation system** (no cloud dependency)
* Understand **diffusion model pipelines deeply**
* Learn **real-world ML environment setup and debugging**
* Create a **modular and extensible system** for future AI integrations

---

## Project Architecture

```
PixelStudio/
│
├── ComfyUI/                 # Core diffusion UI engine
│   ├── models/
│   │   ├── checkpoints/     # Main models (SDXL, JuggernautXL)
│   │   ├── loras/           # Style adapters
│   │   └── controlnet/      # Advanced control models
│
├── outputs/                 # Generated images
├── workflows/               # Saved ComfyUI pipelines
├── notes/                   # Learning + documentation
│
├── requirements.txt         # Clean environment
├── requirements.lock.txt    # Exact environment snapshot
```

---

## Tech Stack

* **Frontend/UI:** ComfyUI (Node-based diffusion interface)
* **Backend:** Python
* **ML Framework:** PyTorch (CUDA-enabled)
* **Models:** Stable Diffusion XL (SDXL), Juggernaut XL
* **Environment:** Conda (Python 3.10)
* **Hardware:** NVIDIA RTX 4050 (6GB VRAM)

---

## Execution Process

### Step 1: Environment Setup

* Created isolated conda environment:

  ```bash
  conda create -n pixelstudio python=3.10
  ```
* Installed CUDA-enabled PyTorch manually using wheel files

---

### Step 2: Dependency Management

Initial approach:

* Used minimal `requirements.txt`

Issues faced:

* Missing dependencies (sqlalchemy, alembic)
* Version conflicts (NumPy 2.x vs PyTorch)

Solution:

* Installed additional dependencies manually
* Stabilized environment with:

  ```bash
  numpy<2
  opencv-python==4.8.1
  ```

---

### Step 3: PyTorch Compatibility Issues

Problem:

* ComfyUI required newer PyTorch APIs (`torch.library.custom_op`)

Attempts:

* Initially installed PyTorch 2.2 → failed
* Upgraded to 2.3 → still incompatible

Final Solution:

* Installed PyTorch 2.4.1 (CUDA 12.1 compatible)

---

### Step 4: ComfyUI Setup

* Cloned repository manually
* Resolved folder conflicts
* Installed dependencies

Issues:

* Missing modules (comfy_kitchen)

Solution:

* Accepted fallback behavior (non-critical module)

---

### Step 5: Model Selection

Challenges:

* Hugging Face models were in **Diffusers format**
* ComfyUI requires **.safetensors checkpoints**

Solution:

* Switched to CivitAI
* Selected **Juggernaut XL (6.6GB)**

---

## Major Problems Faced

### 1. Slow Package Installation

* Pip downloads were extremely slow
* Solution: Manual wheel downloads via browser

---

### 2. NumPy Version Conflict

* NumPy 2.x broke PyTorch compatibility

Solution:

```bash
pip install "numpy<2"
```

---

### 3. OpenCV Compatibility Issue

* New OpenCV required NumPy 2.x

Solution:

```bash
pip install opencv-python==4.8.1.78
```

---

### 4. PyTorch Version Mismatch

* Older versions lacked required APIs

Solution:

* Upgraded to PyTorch 2.4.1

---

### 5. ComfyUI Dependency Explosion

* New updates introduced database + backend layers

Solution:

* Installed full ComfyUI requirements
* Maintained custom minimal requirements

---

### 6. Model Format Confusion

* HuggingFace vs CivitAI formats mismatch

Solution:

* Use `.safetensors` checkpoints for ComfyUI

---

## Key Learnings

* ML environments are **fragile and version-sensitive**

* Always prefer **stable versions over latest**

* Local AI systems require:

  * GPU awareness
  * memory optimization
  * dependency control

* Diffusion systems are not just “prompt → image”
  → They are **pipelines (latent space transformations)**

---

## How to Run PixelStudio

```bash
conda activate pixelstudio
cd ComfyUI
python main.py
```

Open:

```
http://127.0.0.1:8188
```

---

## First Image Generation

Recommended settings:

* Resolution: 768x768
* Steps: 20–25
* Sampler: DPM++ 2M
* CFG: 7

Example prompt:

```
ultra realistic portrait, cinematic lighting, 85mm lens, detailed skin texture
```

---

## Future Scope

* Integrate FastAPI backend (like Local LLM Server)
* Add LoRA support for style customization
* Implement ControlNet pipelines
* Build dataset generation pipelines
* Create UI abstraction layer over ComfyUI

---

## Conclusion

PixelStudio is not just an image generator — it is a **fully functional local AI pipeline** that demonstrates real-world challenges in ML system setup, debugging, and deployment.

This project bridges the gap between:

* theoretical AI knowledge
* and practical system-level implementation

---

## Author

Aditya Guha
AI & ML Enthusiast

---

## Final Note

> "If you can set up a diffusion system locally and debug it — you’re no longer a beginner in AI systems."
