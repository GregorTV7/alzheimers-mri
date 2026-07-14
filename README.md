# alzheimers-mri
Dataset can be found and downloaded at https://www.kaggle.com/datasets/mohiburrahmanrifat/alzheimer
# Prerequisites
docker
python3
torch
kaggle dataset
jetson-inference
resnet and imagenet
# How to Run

1. Install all prerequisites and the kaggle dataset.

2. Unzip the kaggle dataset to jetson-inference/python/training/classification/data

3. Rename alzheimers-split to alzheimers and delete the extra folder

5.  Change directory using cd back into jetson-inference

6. Run the docker container in there ./docker/run.sh

7. Once inside the docker do cd python/training/classification

8.  Make sure labels.txt is with the dataset and tain.py is in python/training/classification

9.  Use train.py to train it python3 train.py --model-dir=models/alzheimers data/alzheimers

10.  This will take awhile, it will stop at 35 epochs and it is ready then

11. Ensure you are in python/training/classification and run the onnx export script python3 onnx_export.py --model-dir=models/alzheimers

12. Exit the docker using Ctrl + D and cd jetson-inference/python/training/classification

13. Set the NET and DATASET values NET=models/alzheimers
DATASET=data/alzheimers

14. Test it on an image of an mri that is very mildly demented imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/Very_Mild_Demented/verymild_12.jpg test.jpg

15. Feel free to test this on other images that use the same format. The result is shown at the end of the command.
