This folder contains Python code
Following are codes and commands that need to be modified and run.
rm -r mslearn-ai-vision -f
git clone https://github.com/MicrosoftLearning/mslearn-ai-vision

cd mslearn-ai-vision/Labfiles/analyze-images/python/image-analysis
ls -a -l


python -m venv labenv
./labenv/bin/Activate.ps1
pip install -r requirements.txt azure-ai-vision-imageanalysis==1.0.0
code .env

code image-analysis.py

# import namespaces
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential

# Authenticate Azure AI Vision client
cv_client = ImageAnalysisClient(
    endpoint=ai_endpoint,
    credential=AzureKeyCredential(ai_key))

# Analyze image
with open(image_file, "rb") as f:
    image_data = f.read()
print(f'\nAnalyzing {image_file}\n')

result = cv_client.analyze(
    image_data=image_data,
    visual_features=[
        VisualFeatures.CAPTION,
        VisualFeatures.DENSE_CAPTIONS,
        VisualFeatures.TAGS,
        VisualFeatures.OBJECTS,
        VisualFeatures.PEOPLE],
)


# Get image captions
if result.caption is not None:
    print("\nCaption:")
    print(" Caption: '{}' (confidence: {:.2f}%)".format(result.caption.text, result.caption.confidence * 100))

if result.dense_captions is not None:
    print("\nDense Captions:")
    for caption in result.dense_captions.list:
        print(" Caption: '{}' (confidence: {:.2f}%)".format(caption.text, caption.confidence * 100))


python image-analysis.py images/street.jpg

# Get image tags
if result.tags is not None:
    print("\nTags:")
    for tag in result.tags.list:
        print(" Tag: '{}' (confidence: {:.2f}%)".format(tag.name, tag.confidence * 100))


# Get objects in the image
if result.objects is not None:
    print("\nObjects in image:")
    for detected_object in result.objects.list:
        # Print object tag and confidence
        print(" {} (confidence: {:.2f}%)".format(detected_object.tags[0].name, detected_object.tags[0].confidence * 100))
    # Annotate objects in the image
    show_objects(image_file, result.objects.list)


download objects.jpg


# Get people in the image
if result.people is not None:
    print("\nPeople in image:")

    for detected_person in result.people.list:
        if detected_person.confidence > 0.2:
            # Print location and confidence of each person detected
            print(" {} (confidence: {:.2f}%)".format(detected_person.bounding_box, detected_person.confidence * 100))
    # Annotate people in the image
    show_people(image_file, result.people.list)

download people.jpg
