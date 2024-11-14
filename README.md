# window-maker-90
Accepts a folder of images in portrait 9:16 and stitches them together to create a 16:9 -90deg display for a window, with each picture staying onscreen for 10 seconds. This is sorted alphabetically, so to extend a single image, duplicate it.

## Ensure you have ffmpeg installed

```
ffmpeg -version
```

## Create a python environment and install the requisites

```
py -m venv .venv
```

```
.venv\Scripts\activate
```

```
pip install opencv-python natsort
```

## Populate the images folder, and create an output folder then run the script

```
mkdir images, output
```

```
./create-window.py
```

### create-window.py

```python
import cv2
import os
from natsort import natsorted
import datetime

def create_video_from_images(image_folder, output_video, fps=0.1):
    images = [img for img in os.listdir(image_folder) if img.endswith(".png") or img.endswith(".jpg")or img.endswith(".jpeg")]
    images = natsorted(images)

    if not images:
        print("No images found in the folder.")
        return

    frame = cv2.imread(os.path.join(image_folder, images[0]))
    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    video = cv2.VideoWriter(output_video, fourcc, fps, (1920, 1080))

    for image in images:
        print(f"Adding {image} to the video")
        img_path = os.path.join(image_folder, image)
        frame = cv2.imread(img_path)
        ResizeTo = (1080, 1920)
        frame = cv2.resize(frame, ResizeTo)
        #rotate -90 degree
        frame = cv2.rotate(frame, cv2.ROTATE_90_COUNTERCLOCKWISE)
        for _ in range(int(fps * 10)):  # Display each image for 10 seconds
            video.write(frame)

    video.release()
    print(f"Video saved as {output_video}")

def generate_datestamp():
    return datetime.datetime.now().strftime("%Y%m%d_%H%M%S")

def rotate_video(input_video):
        output_video = input_video.replace(".mp4", "_rotated.mp4")
        os.system(f'ffmpeg -i {input_video} -vf "transpose=2" {output_video}')
        print(f"Rotated video saved as {output_video}")

if __name__ == "__main__":
    image_folder = "images"
    output_folder = "output"
    datestamp = generate_datestamp()
    output_video = os.path.join(output_folder, f"window_gen_{datestamp}.mp4")
    # Original aspect ratio: 9:16, will output as -90 rotated 16:9
    create_video_from_images(image_folder, output_video)

```
