rm -r mslearn-ai-vision -f
git clone https://github.com/MicrosoftLearning/mslearn-ai-vision

cd mslearn-ai-vision/Labfiles/face/python/face-api
ls -a -l

python -m venv labenv
./labenv/bin/Activate.ps1
pip install -r requirements.txt azure-ai-vision-face==1.0.0b2;\

code .env
code analyze-faces.py

# Import namespaces
from azure.ai.vision.face import FaceClient
from azure.ai.vision.face.models import FaceDetectionModel, FaceRecognitionModel, FaceAttributeTypeDetection01
from azure.core.credentials import AzureKeyCredential


# Authenticate Face client
face_client = FaceClient(
    endpoint=cog_endpoint,
    credential=AzureKeyCredential(cog_key))

# Specify facial features to be retrieved
features = [FaceAttributeTypeDetection01.HEAD_POSE,
            FaceAttributeTypeDetection01.OCCLUSION,
            FaceAttributeTypeDetection01.ACCESSORIES]

# Get faces
with open(image_file, mode="rb") as image_data:
    detected_faces = face_client.detect(
        image_content=image_data.read(),
        detection_model=FaceDetectionModel.DETECTION01,
        recognition_model=FaceRecognitionModel.RECOGNITION01,
        return_face_id=False,
        return_face_attributes=features,
    )

face_count = 0
if len(detected_faces) > 0:
    print(len(detected_faces), 'faces detected.')
    for face in detected_faces:

        # Get face properties
        face_count += 1
        print('\nFace number {}'.format(face_count))
        print(' - Head Pose (Yaw): {}'.format(face.face_attributes.head_pose.yaw))
        print(' - Head Pose (Pitch): {}'.format(face.face_attributes.head_pose.pitch))
        print(' - Head Pose (Roll): {}'.format(face.face_attributes.head_pose.roll))
        print(' - Forehead occluded?: {}'.format(face.face_attributes.occlusion["foreheadOccluded"]))
        print(' - Eye occluded?: {}'.format(face.face_attributes.occlusion["eyeOccluded"]))
        print(' - Mouth occluded?: {}'.format(face.face_attributes.occlusion["mouthOccluded"]))
        print(' - Accessories:')
        for accessory in face.face_attributes.accessories:
            print('   - {}'.format(accessory.type))
        # Annotate faces in the image
        annotate_faces(image_file, detected_faces)

python analyze-faces.py images/face1.jpg

download detected_faces.jpg

python analyze-faces.py images/face2.jpg

download detected_faces.jpg

python analyze-faces.py images/faces.jpg

download detected_faces.jpg

