This folder contains Python code

cd mslearn-ai-vision/Labfiles/ocr/python/read-text
ls -a -l

python -m venv labenv
./labenv/bin/Activate.ps1
pip install -r requirements.txt azure-ai-vision-imageanalysis==1.0.0

code .env

code read-text.py

# import namespaces
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential

# Authenticate Azure AI Vision client
cv_client = ImageAnalysisClient(
    endpoint=ai_endpoint,
    credential=AzureKeyCredential(ai_key))


# Read text in image
with open(image_file, "rb") as f:
    image_data = f.read()
print (f"\nReading text in {image_file}")

result = cv_client.analyze(
    image_data=image_data,
    visual_features=[VisualFeatures.READ])

# Print the text
if result.read is not None:
    print("\nText:")

    for line in result.read.blocks[0].lines:
        print(f" {line.text}")        
    # Annotate the text in the image
    annotate_lines(image_file, result.read)

    # Find individual words in each line

python read-text.py images/Lincoln.jpg

download lines.jpg

python read-text.py images/Business-card.jpg

download lines.jpg

python read-text.py images/Note.jpg

download lines.jpg

# Find individual words in each line
print ("\nIndividual words:")
for line in result.read.blocks[0].lines:
    for word in line.words:
        print(f"  {word.text} (Confidence: {word.confidence:.2f}%)")
# Annotate the words in the image
annotate_words(image_file, result.read)

download words.jpg
