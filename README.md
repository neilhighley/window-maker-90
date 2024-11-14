# window-maker-90
Accepts a folder of images in portrait 9:16 and stitches them together to create a 16:9 -90deg display for a window, with each picture staying onscreen for 10 seconds. This is sorted alphabetically, so to extend a single image, duplicate it.

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
