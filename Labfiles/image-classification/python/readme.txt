This folder contains Python code

https://customvision.ai

Classify Fruit
Image classification for fruit

https://github.com/MicrosoftLearning/mslearn-ai-vision/raw/main/Labfiles/image-classification/training-images.zi

https://aka.ms/test-apple
https://aka.ms/test-banana

Project Id:

End Point & Key

Git hub repo:
rm -r mslearn-ai-vision -f
git clone https://github.com/MicrosoftLearning/mslearn-ai-vision

cd mslearn-ai-vision/Labfiles/image-classification/python/train-classifier
ls -a -l

python -m venv labenv
./labenv/bin/Activate.ps1
pip install -r requirements.txt azure-cognitiveservices-vision-customvision

code .env
code train-classifier.py
python train-classifier.py


Publish the image classification model

fruit-classifier

Resources End point & Key


In Az Cloud Shell
cd ../test-classifier
ls -a -l

python -m venv labenv
./labenv/bin/Activate.ps1
pip install -r requirements.txt azure-cognitiveservices-vision-customvision
code .env
code test-classifier.py
python test-classifier.py
