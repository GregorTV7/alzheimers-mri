# Alzheimer's MRI Classification

A ResNet18 image classifier trained on brain MRI scans to detect signs of Alzheimer's disease, built using NVIDIA's `jetson-inference` framework.

Dataset: [Alzheimer MRI Dataset on Kaggle](https://www.kaggle.com/datasets/mohiburrahmanrifat/alzheimer)

## Prerequisites

- Docker
- Python 3
- PyTorch
- Kaggle dataset (linked above)
- [jetson-inference](https://github.com/dusty-nv/jetson-inference)
- ResNet18 and ImageNet

## How to Run

1. Install all prerequisites and download the Kaggle dataset.
2. Unzip the dataset into `jetson-inference/python/training/classification/data`.
3. Rename the `alzheimers-split` folder to `alzheimers` and delete the extra nested folder.
4. `cd` back into `jetson-inference`.
5. Run the Docker container:
   ```bash
   ./docker/run.sh
   ```
6. Inside the container, change directory:
   ```bash
   cd python/training/classification
   ```
7. Make sure `labels.txt` is included with the dataset and `train.py` is in `python/training/classification`.
8. Train the model:
   ```bash
   python3 train.py --model-dir=models/alzheimers data/alzheimers
   ```
   Training will run for 35 epochs — once it stops, the model is ready.
9. Export the model to ONNX (still inside the container, in `python/training/classification`):
   ```bash
   python3 onnx_export.py --model-dir=models/alzheimers
   ```
10. Exit the container with `Ctrl+D`, then `cd` into `jetson-inference/python/training/classification`.
11. Set the model and dataset paths:
    ```bash
    NET=models/alzheimers
    DATASET=data/alzheimers
    ```
12. Test the model on a "very mildly demented" MRI image:
    ```bash
    imagenet.py --model=$NET/resnet18.onnx \
                 --input_blob=input_0 \
                 --output_blob=output_0 \
                 --labels=$DATASET/labels.txt \
                 $DATASET/test/Very_Mild_Demented/verymild_12.jpg test.jpg
    ```
13. Feel free to test on other images in the same format, results are printed at the end of the command, It is better to train it with more epochs, even mroe than 35, you can do this by editing train.py and you should get better results.

14. After the test in step 12 it should look like this. In this example it was less accurate since it was only 35 epochs.

![Screenshot](https://raw.githubusercontent.com/GregorTV7/alzheimers-mri/main/after.jpg)

![Screenshot](https://raw.githubusercontent.com/GregorTV7/alzheimers-mri/main/screenshot.png)

## Purpose & Demonstration Video

[![demonstration.mp4](https://raw.githubusercontent.com/your-username/your-repo/main/path-to-thumbnail.png)](https://drive.google.com/file/d/1zB4pneeE9ofaDWzEmLfmvqqivpqZQDSL/view?usp=sharing)

## Acknowledgments

Dataset provided by [Mohibur Rahman Rifat](https://www.kaggle.com/mohiburrahmanrifat) on Kaggle:
[Alzheimer MRI Dataset](https://www.kaggle.com/datasets/mohiburrahmanrifat/alzheimer)
