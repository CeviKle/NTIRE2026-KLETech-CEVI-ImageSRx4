# NTIRE 2026 Image Super-Resolution Challenge - Fact Sheet

## 1. Team details
- **Team name:** Team 26
- **Team leader name:** User/Antigravity
- **Team leader address, phone number, and email:** (Please fill before final submission)
- **Rest of the team members:** (Please fill)
- **Team website URL:** N/A
- **Affiliation:** N/A

## 2. Contribution details
- **Title of the contribution:** Enhanced SwinIR (180) for Robust Image Super-Resolution
- **General method description:** 
We utilize a higher capacity variant of the Swin Transformer for Image Restoration (SwinIR). The model scales the embedding dimension to 180 and uses 6 Residual Swin Transformer Blocks (RSTB), each featuring 6 Swin Transformer Layers. The network is trained using a micro-batch configuration coupled with a stable composite Charbonnier + 0.01 * VGG Perceptual loss. We apply an Exponential Moving Average (EMA) with a decay of 0.999 to stabilize final evaluation weights.
- **Representative image / diagram of the method:** (Please attach to final package if needed)

## 3. Global Method Description
- **Total method complexity:** ~11.90 Million Parameters. High MACs relative to base SwinIR due to 180 embed_dim.
- **Which pre-trained or external methods / models have been used?** 
The model is initialized via a pre-training phase using only the DF2K dataset, followed by a concentrated fine-tuning phase on the same dataset with a perceptually aligned loss. No internal dataset augmentations outside standard rotations/flips were employed. VGG19 was used strictly to compute the perceptual loss.

## 4. Training description
- **Training dataset:** DF2K (DIV2K Training + Flickr2K) - Total 3,450 images.
- **Training details:**
    - Patch size (LR): 48x48
    - Batch size: 2
    - Optimizer: Adam (Initial LR: 2e-4 pretrained, 5e-5 fine-tuned)
    - Scheduler: Cosine Annealing to minimum LR of 1e-7
    - Loss: Charbonnier Loss + 0.01 * VGG19 (relu5_4) Perceptual Loss
    - Hardware: Trained on standard GPU
    - Fine-tuning iterations: 50,000
    - Exponential Moving Average: Decay 0.999 used for inference weights representation.

## 5. Other details
- **Language / framework:** Python / PyTorch
- **Execution time:** ~4.5 seconds per image (CPU fallback noted, lower on dedicated modern GPU setups).
