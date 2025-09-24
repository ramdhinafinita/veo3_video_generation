# Veo 3 - AI Video Generation Samples

[![Run in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPO/blob/main/veo_3_example.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-blue?logo=github)](https://github.com/YOUR_USERNAME/YOUR_REPO)

This repository provides sample code and reusable templates for getting started with **Veo 3**, Google's most advanced AI video generation model. Use these examples as a base to reproduce, experiment, and create your own high-quality videos from text and image prompts.

### Important Links
*   **Veo 3 Prompts & Demos:** [https://aiprompts.video/veo3](https://aiprompts.video/veo3)
*   **Veo 3 Official Website:** [https://deepmind.google/models/veo/](https://deepmind.google/models/veo/)

---

## 🎬 What is Veo 3?

Veo 3 is a cutting-edge video generation model from Google that creates high-quality, 1080p+ videos with stunning detail and realistic physics across a wide array of visual styles. It excels at understanding natural language and cinematic terminology, allowing it to produce videos that closely match the user's creative vision. This version introduces the ability to generate synchronized audio, including dialogue, sound effects, and music.

### 🔍 Key Features

*   **Multimodal Input**: Generates videos from text, images, or a combination of both.
*   **High-Quality Output**: Produces videos over a minute long in 1080p resolution, with experimental capabilities for longer outputs.
*   **Audio Integration**: Natively generates soundtracks with realistic dialogue, sound effects, and ambient noise that matches the video content.
*   **Advanced Realism**: Superior understanding of real-world physics, character consistency, and accurate lip-syncing.
*   **Narrative Understanding**: Can interpret complex prompts to create coherent storylines and cinematic shots like timelapses or aerial views.

---

## 🚀 Getting Started

To use this code, you will need access to the Vertex AI API on Google Cloud.

### Prerequisites

1.  A Google Cloud project with billing enabled.
2.  The **Vertex AI API** enabled in your project.
3.  Authenticated environment (e.g., via Google Colab or `gcloud auth`).

### 1. Installation

Install the necessary Python SDK:
```bash
pip install --upgrade --quiet google-genai
```

### 2. Authentication & Configuration

To run the notebook, you'll need to authenticate your environment and configure the client with your Google Cloud project details.

**If using Google Colab, first authenticate your user:**
```python
import sys

if "google.colab" in sys.modules:
    from google.colab import auth
    auth.authenticate_user()
```

**Then, import libraries and set up the client:**
```python
import os
import time
from IPython.display import Video, display
from google import genai
from google.genai import types

# Replace with your Project ID
PROJECT_ID = "[your-project-id]"
LOCATION = "us-central1" # Or your preferred location

if not PROJECT_ID or PROJECT_ID == "[your-project-id]":
    PROJECT_ID = str(os.environ.get("GOOGLE_CLOUD_PROJECT"))

client = genai.Client(vertexai=True, project=PROJECT_ID, location=LOCATION)

# Load the video generation models
video_model = "veo-3.0-generate-001"
image_video_model = "veo-3.0-generate-preview"
```

---

## 💻 Usage Examples

This repository demonstrates how to generate videos from both text and image prompts.

### 1. Generate Video from Text

You can create a video from a detailed text description. For best results, use a structured prompt that includes elements like subject, action, scene, camera work, and audio.

This example uses the Gemini model to build a highly-optimized prompt from keywords.

```python
# 1. Define keywords for your video
subject = "a detective"
action = "interrogating a rubber duck"
scene = "in a dark interview room"
camera_angle = "Over-the-Shoulder Shot"
camera_movement = "Zoom (In)"
style = "Cinematic"
sound_effects = "Ticking clock"
dialogue = "Where were you last night?"

# 2. Use Gemini to synthesize an optimal prompt (optional, but recommended)
# (See the full notebook for the prompt generation logic)
# Let's assume Gemini generates the following prompt:
prompt = "Cinematic over-the-shoulder shot zooming in on a detective interrogating a rubber duck in a dark interview room. A ticking clock is heard, and the detective asks, 'Where were you last night?'"

print(prompt)

# 3. Send the video generation request
operation = client.models.generate_videos(
    model=video_model,
    prompt=prompt,
    config=types.GenerateVideosConfig(
        aspect_ratio="16:9",
        number_of_videos=1,
        duration_seconds=6,
        resolution="1080p",
        enhance_prompt=True,
        generate_audio=True,
    ),
)

# 4. Poll for completion
while not operation.done:
    print("Waiting for video generation to complete...")
    time.sleep(15)
    operation = client.operations.get(operation)

# 5. Display the result
if operation.response:
    # Assuming 'show_video' is a helper function to display the video
    show_video(operation.result.generated_videos[0].video.video_bytes)```

### 2. Generate Video from an Image

You can also use a starting image to guide the video generation process, adding motion and animation.

```python
# 1. Define the path to your starting image
starting_image = "path/to/your/image.png"

# 2. Define keywords for motion and audio
camera_motion = "Zoom (Out)"
environmental_animation = "Leaves rustle in the wind"
sound_effects = "Soft ambient sounds of a forest"

# 3. Use Gemini to generate a prompt based on the image and keywords (optional)
# (See the full notebook for the prompt generation logic)
# Let's assume Gemini generates the following prompt:
prompt = "A slow zoom out from the flowers, as leaves gently rustle in the wind, accompanied by the soft ambient sounds of a forest."

print(prompt)

# 4. Send the image-to-video generation request
operation = client.models.generate_videos(
    model=image_video_model,
    prompt=prompt,
    image=types.Image.from_file(location=starting_image),
    config=types.GenerateVideosConfig(
        aspect_ratio="16:9",
        duration_seconds=6,
        resolution="1080p",
        generate_audio=True,
    ),
)

# 5. Poll for completion and display the result (same as above)
while not operation.done:
    time.sleep(15)
    operation = client.operations.get(operation)

if operation.response:
    show_video(operation.result.generated_videos[0].video.video_bytes)
```

## 🤝 Contributing

Contributions are welcome! Feel free to fork the repository, create a new branch, and submit a pull request with your improvements or additional examples.

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

## 📄 License

This project is licensed under the Apache 2.0 License. See the `LICENSE` file for more details.
