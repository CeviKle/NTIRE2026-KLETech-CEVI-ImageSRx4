# NTIRE 2026 Challenge on Image Super-Resolution (x4) - Team 26 (SwinIR_180)

This repository contains the official code submission for **Team 26** in the **NTIRE 2026 Image Super-Resolution (x4) Challenge**. Our solution is based on an enhanced, high-capacity variant of the **SwinIR** (Swin Transformer for Image Restoration) architecture.

Our model achieves highly competitive results by scaling the embedding dimensions to 180 and fine-tuning with a composite Charbonnier and VGG Perceptual loss on the DF2K dataset.

---

## 🚀 1. Environment Requirements

To ensure full compatibility and reproducibility, we recommend running this code in an isolated Anaconda environment.

* **Operating System**: Linux (Ubuntu 20.04/22.04 recommended) or Windows
* **Hardware**: GPU with at least 12GB VRAM (e.g., NVIDIA RTX 3080/4090 or A100/V100)
* **Software**:
  * Python >= 3.8
  * PyTorch >= 1.13.0 (with CUDA support)

---

## 🛠️ 2. Installation Instructions

Follow these steps to set up the environment and prepare the repository:

**Step 1: Clone this repository**
```bash
git clone https://github.com/CeviKle/NTIRE2026-KLETech-CEVI-ImageSRx4.git
cd NTIRE2026-KLETech-CEVI-ImageSRx4
```

**Step 2: Create and activate a Conda environment**
```bash
conda create -n SwinIR_NTIRE python=3.10 -y
conda activate SwinIR_NTIRE
```

**Step 3: Install PyTorch**
*(Please adjust the CUDA version based on your local machine configuration. The following is for CUDA 11.8)*
```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
```

**Step 4: Install required dependencies**
```bash
pip install -r requirements.txt
```
*(If `requirements.txt` is missing, you can manually install the required libraries:)*
```bash
pip install opencv-python numpy tqdm matplotlib timm
```

---

## ⚙️ 3. Pretrained Model Weights

The fine-tuned model checkpoint achieving the submitted results is already included in this repository.
* **Path:** `model_zoo/team26_SwinIR_180/best_model.pth`
* **Note:** You do not need to download any external weights. The submission package is fully self-contained.

---

## 🏃 4. Running the Inference (Testing)

We use the official benchmark script `test.py` provided by the challenge organizers. Our specific model is accessed by passing the argument `--model_id 26`.

### Command to Restore Images:

To run inference on your folder of Low-Resolution (LR) images, execute the following command from the root of the repository:

```bash
CUDA_VISIBLE_DEVICES=0 python test.py \
    --test_dir /absolute/path/to/your/LR_images \
    --save_dir ./results \
    --model_id 26
```

### Parameter Breakdown:
* `CUDA_VISIBLE_DEVICES=0`: Specifies which GPU to use (use `0,1` for multi-GPU).
* `--test_dir`: **[Replace this]** The path to the directory containing your Low-Resolution (LQ) input images.
* `--save_dir`: The directory where the high-resolution output images will be stored (defaults to `./results`).
* `--model_id 26`: **[CRITICAL]** This tells the script to load our custom `Team26_SwinIR` model and its corresponding pretrained weights.

**Expectation:**
The script will sequentially process each image in your `--test_dir`. You will see a `tqdm` progress bar in the terminal, and the final 4x upscaled images will be saved directly to the `--save_dir`. The script automatically handles necessary border padding for the Swin Transformer's attention windows.

---

## 📊 5. Model Architecture & Fact Sheet Summary

Our submitted model is a high-capacity SwinIR explicitly configured for 4x Super-Resolution.

* **Architecture:** Swin Transformer for Image Restoration (SwinIR)
* **Embedding Dimension:** 180 (Increased from baseline for richer feature extraction)
* **Depth (RSTB Layers):** 6 blocks, each with 6 Swin Transformer layers `[6, 6, 6, 6, 6, 6]`
* **Heads:** `[6, 6, 6, 6, 6, 6]`
* **Window Size:** 8x8 pixels
* **MLP Ratio:** 2.0
* **Upsampler:** PixelShuffle
* **Total Parameters:** ~11.90 Million
* **Training Data:** DF2K (DIV2K Training Set + Flickr2K)
* **Validation Performance:** Average PSNR of **28.9705 dB** and SSIM of **0.8216** on the DIV2K validation set.

For more profound details on the training paradigm, loss functions (Charbonnier + 0.01* VGG Perceptual), and hyperparameter scheduling, please refer to the `Fact_Sheet.md` included in this repository.

---

## 📂 6. Repository Structure

```text
NTIRE2026-KLETech-CEVI-ImageSRx4/
├── models/
│   ├── network_swinir.py       # Core SwinIR architecture definition
│   ├── team26_SwinIR.py        # Wrapper script for handling padding & model instantiation
│   └── ... 
├── model_zoo/
│   └── team26_SwinIR_180/
│       └── best_model.pth      # ★ The fine-tuned 11.9M parameter checkpoint 
├── test.py                     # Official evaluation script (patched to support --model_id 1)
├── Fact_Sheet.md               # Detailed methods and training descriptions
├── README.md                   # This file
└── ...
```

---

## 📜 7. License and Acknowledgements

This codebase expands upon the official NTIRE repository and the original [SwinIR](https://github.com/JingyunLiang/SwinIR) implementation. This repository is released under the MIT License.
