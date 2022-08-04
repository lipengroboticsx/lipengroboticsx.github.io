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

### Test

now you should be able to run the following command to launch the event recorder with your Prophesee event camera connected to the computer:

```p
metavision_viewer
```

You should also be able to record using ZED by running the official [sample](https://github.com/stereolabs/zed-examples/tree/master/svo%20recording/recording/python). If you don't have a camera or don't intend to record, you could just check if `pyzed` and `metavision_core` modules can be successfully imported in your python program. If any failure, you should inspect your installation if done manually and, unfortunately, troubleshooting this is beyond the scope of this instruction.

 

## Data Processing

Our data processing script converts the raw data to the format as described in [Processed Data](https://lipengroboticsx.github.io/dataset/). To run the script, you have to **first** put the raw data of each recording into an individual folder named by the recording ID under the directory `/data` and organize the data from different sensors as displayed in [Raw Data](https://lipengroboticsx.github.io/dataset/). This sorting can be much effortless if the raw data is recorded using our recorder program since they will be produced in a way ready to be processed.

**Second**, once the data organized appropriately, all you need to do is just running the following command:

```p
python src/postprocess.py
```



## Recorder

Our recorder integrates the functionality of arranging the content to be recorded, recording with multiple devices, and annotating the result of the recording into one user-friendly interactive program. 

**First**, enable all recording devices and ensure each of them function smoothly. Three ZED cameras and one Prophesee event camera should be wired to the host where the recorder program is supposed to run. StretchSense MoCap Pro gloves should be wireless connected to a Windows machine with its official client software Hand Engine running. OptiTrack server can be either operated on a separate host, recommended by us, or on the same host as any of the two aforementioned ones a.l.a. the computational resource allows and the performance will not be thus compromised. You may need to configure the firewall on each machine to allow the smooth (UDP) communication among them.

**Second**, update the configuration in our OptiTrack NatNet client code and rebuild the NatNet client. You need to set the values of OptiTrack server IP address (TODO Variable Name), recorder IP address (TODO Variable Name), and recorder port (TODO Variable Name) according to your network setting in the file `/recording/Optitrack_NatNet_client/src/example_main.cpp`.  TODO rebuild instruction

```p

```

**Third**, initialize your lists of subjects and objects in the corresponding files `/register/subjects.csv` and `/register/objects.csv` respectively. Each subject and object should lie in a new line. Please check the sample lists in our repository for detailed format.

**Next**, launch the main recorder application:

```

```

and the NatNet client:

```

```

now you should be able to see the prompt indicating that these two applications have successfully communicated with each other, if everything goes well, as shown blow 

TODO pictures of connection established.

**Last**, operate the main recorder to record following the interactive instruction. The main recorder will automatically communicate with and command Hand Engine and NatNet client to record. Nevertheless, we do recommend you to regularly check Hand Engine and NatNet client to see if bug.

TODO picture of a complete take

## Annotator

To annotate, you should **first** have the processed data under the directory `/data/recording_id/processed`. The processed data can be downloaded from the selected [processed sampled](https://lipengroboticsx.github.io/dataset/) or obtained by processing the raw data as suggested in [Data Processing](#data-processing).  **Next**, you run the following command to launch the annotation application. Note that our annotation application is independent of ZED SDK and Metavision SDK, so you can use the application without them installed.

```p
python src/annotate.py
```

Now you should be able to see the following interface:

![](https://raw.githubusercontent.com/lipengroboticsx/lipengroboticsx.github.io/main/assets/images/annotation_tool.png)

On the left top of each sub-window, the information of the current frame is given as:

 `{current frame number} / {total number of frames} month:day hour:minute:second:millisecond`

Inside the information panel (bottom right sub-window), the annotation result is displayed in real time. This includes the manual annotation and the annotation automatically generated by the program.

![](https://raw.githubusercontent.com/lipengroboticsx/lipengroboticsx.github.io/main/assets/images/info_panel_explanation.png)

1. the status of the annotation: finished, unfinished or problematic
2. which hand used to throw at moment *throw*
3. the position of the thrower at moment *throw*
4. the position of the catcher at moment *throw*
5. the average flying speed of the thrown object
6.  the vertical hand position of the thrower at moment *throw*
7. the horizontal hand position of the thrower at moment *throw*
8. the vertical hand position of the catcher at moment *throw*
9. the horizontal hand position of the catcher at moment *throw*
10. the frame number and the timestamp of the moment *throw*
11. which hand used to catch at moment *catch (stable)* 
12. the position of the catcher at moment *catch (touch)*
13. the vertical hand position of the catcher at moment *catch (stable)*
14. the horizontal hand position of the catcher at moment *catch (stable)*
15. the frame number and the timestamp of the moment *catch (touch)*
16. the frame number and the timestamp of the moment *catch (stable)*



The keys to operate the application are defined as:

* **right arrow**: next frame
* **left arrow**: last frame
* **down arrow**: next recording
* **up arrow**: last recording
* **return**: take the current frame as a moment of throw, catch (touch), and catch (stable) in order
* **del**: remove the last annotated moment
* **Q**: switch the values of info panel 2 among left, right, and both 
* **A**: switch the values of info panel 6 among overhead, overhand, chest, and underhand
* **S**: switch the values of info panel 7 among left, middle, and right
* **D**: switch the values of info panel 8 among overhead, overhand, chest, and underhand
* **F**: switch the values of info panel 9 among left, middle, and right
* **Z**: switch the values of info panel 11 among left, right, and both
* **C**: switch the values of info panel 2 among overhead, overhand, chest, and underhand
* **V**: switch the values of info panel 2 among left, middle, and right
* **space**: switch the values of info panel 1 between finished and unfinished
* **backspace**: switch the values of info panel 1 between problematic and unfinished

All modification to the annotation result will be immediately saved in the corresponding annotation file under the directory `/annotations/recording_id.json`.

