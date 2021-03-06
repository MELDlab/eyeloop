# Importers #
<p align="right">
    <img src="https://github.com/simonarvin/eyeloop/blob/master/misc/imgs/importer_overview.svg?raw=true" align="right" height="250">
    </p>
    
To use a video sequence for eye-tracking, we use an *importer* class as a bridge to EyeLoop's engine. The importer fetches the video sequence from the camera, or offline from a directory, and imports it. Briefly, the importer main class ```IMPORTER``` includes functions to rotate, resize and save the video stream. Additionally, it *arms* the engine by passing neccesary variables.

## Why use an importer? ##
The reason for using import modules, rather than having it "built-in", is to prevent incompatibilities. For example, while most web-cameras are compatible with opencv (importer *cv*), Vimba-based cameras, such as Allied Vision cameras, are not. Thus, by modularizing the importation of image frames, EyeLoop is easily integrated in markedly different setups.

## Building your first custom importer ##
To build our first custom importer, we instantiate our Importer class:
```python
class Importer(IMPORTER):
    def __init__(self, ENGINE) -> None:
        self.ENGINE = ENGINE
        self.arguments = ENGINE.arguments
        self.scale = self.arguments.scale
```
Here, we define critical variables, such as optional scaling. Then, we load the first frame, retrieve its dimensions and, lastly, arm the engine:

```python
        ...
        (load image)
        width, height = (image dimensions)
        self.arm(width, height, image)
```
Finally, the ```route()``` function loads the subsequent frames and passes them to the engine sequentially:
```python
def route(self) -> None:
        while True:
            image = ...
            self.ENGINE.update_feed(image)
            self.frame += 1
```
Optionally, add a ```release()``` function to control termination of the importation process:
```python
def release(self) -> None:
        terminate()
```
