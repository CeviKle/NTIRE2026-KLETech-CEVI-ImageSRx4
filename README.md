# NTIRE 2026 Challenge on Image Super-Resolution (x4) - Team 01 (SwinIR_180)

This repository contains the SwinIR_180 model fine-tuned for the NTIRE 2026 Image Super-Resolution (x4) challenge.

## Environment Requirements
The code was tested with the following environment:
- Python 3.8+
- PyTorch (compatible with standard CUDA environments)
- OpenCV (`cv2`)
- numpy
- tqdm

**Installation:**
```bash
pip install torch torchvision opencv-python numpy tqdm
```

## How to Test the Model

To run the model on your dataset, use the official `test.py` script with `--model_id 1`.

```bash
CUDA_VISIBLE_DEVICES=0 python test.py \
    --test_dir [path_to_LQ_images] \
    --save_dir [path_to_save_HR_images] \
    --model_id 1
```

- `--test_dir`: Absolute or relative path to the folder containing LR images.
- `--save_dir`: Directory where the restored images will be saved.
- `--model_id 1`: Specifically calls the `SwinIR_180` model.

## Model Configuration
This model is based on the SwinIR architecture optimized for x4 upscaling:
- Embed dimension: 180
- Depths: [6, 6, 6, 6, 6, 6]
- Number of heads: [6, 6, 6, 6, 6, 6]
- Window size: 8
- MLP Ratio: 2.0
- Total Parameters: ~11.90 Million
