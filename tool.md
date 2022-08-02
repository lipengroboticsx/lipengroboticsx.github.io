We have developed three tools to [process](#data-processing) the raw data, [record](#recorder) and [annotate](#annotator) a throw-catch activity. All source code has been published on GitHub. TODO github link

## Dependencies

To run our code, some dependencies have to be installed. First, the default, and well-tested, development environment is

* Ubuntu: 20.04
* Python: 3.8.13
* CUDA: TODO
* Nvidia driver: 510

We have not tested our code on other development environments, so you are recommended to configure the same, or at least similar, environment for the best experience.

### Manual Installation

Second, you need to install [ZED SDK](https://www.stereolabs.com/docs/installation/) (3.7.6) and [Metavision SDK](https://docs.prophesee.ai/stable/installation/index.html) (TODO) following the official guidance in the links in order to record and process the data of ZED and Prophesee Event cameras respectively. For Metavision SDK, the machine learning modules are not required in our code.

Last, the remaining dependencies include

* addict
* mttkinter
* openpyxl
* scipy
* pandas
* opencv-python

and can be automatically installed via pip:

```p
pip install -r requirements.txt
```

### Docker

Alternatively, we also provide a ready-to-use [Docker](https://www.docker.com/) with all dependencies installed in our git repository (TODO link). To use it, Docker should have been already successfully installed.



## Data Processing

data processing



## Recorder

recording a session



## Annotator

data annotation