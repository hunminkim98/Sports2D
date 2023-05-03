<!-- 
[![Continuous integration](https://github.com/davidpagnon/sports2d/actions/workflows/continuous-integration.yml/badge.svg?branch=main)](https://github.com/davidpagnon/sports2d/actions/workflows/continuous-integration.yml)
[![PyPI version](https://badge.fury.io/py/Sports2D.svg)](https://badge.fury.io/py/Sports2D) \
[![Downloads](https://pepy.tech/badge/sports2d)](https://pepy.tech/project/sports2d)
[![Stars](https://badgen.net/github/stars/davidpagnon/sports2d)](https://github.com/davidpagnon/sports2d/stargazers)
[![License](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)
[![GitHub issues](https://img.shields.io/github/issues/davidpagnon/sports2d)](https://github.com/davidpagnon/sports2d/issues)
[![GitHub issues-closed](https://img.shields.io/github/issues-closed/davidpagnon/sports2d)](https://GitHub.com/davidpagnon/sports2d/issues?q=is%3Aissue+is%3Aclosed) 
-->


# Sports2D

**`Sports2D` lets you compute 2D joint and segment angles from a single video.**

 </br>
 </br>

<img src='Content/demo_gif.gif' title='Demonstration of Sports2D with OpenPose.'  width="760">

`Warning:` Angle estimation is only as good as the pose estimation algorithm, i.e., it is not perfect.\
`Warning:` Results are acceptable only if the persons move in the 2D plane, from right to left or from left to right.\
If you need research-grade markerless joint kinematics, consider using several cameras, and constraining angles to a biomechanically accurate model. See [Pose2Sim](https://github.com/perfanalytics/pose2sim) for example.

`Announcement:` Apps with GUI will be released for Windows, Linux, MacOS, as well as Android and iOS.
Mobile versions will only support simple exploratory analysis. This involves single-person angle computation, in a potentially less accurate and tunable way.


## Contents
1. [Installation and Demonstration](#installation-and-demonstration)
   1. [Installation](#installation)
   2. [Demonstration: Detect pose and compute 2D angles](#demonstration-detect-pose-and-compute-2d-angles)
2. [Go further](#go-further)
   1. [Use on your own videos](#use-on-your-own-videos)
   2. [Use OpenPose for multi-person, more accurate analysis](#use-openpose-for-multi-person-more-accurate-analysis)
   3. [Advanced-settings](#advanced-settings)
   4. [How it works](#how-it-works)
3. [How to cite and how to contribute](#how-to-cite-and-how-to-contribute)


## Installation and Demonstration

### Installation

- OPTION 1: **Quick install:** \
    Open a terminal. Type `python -V` to make sure python '>=3.7 <=3.10' is installed, and then:
    ```
    pip install sports2d
    ```

- OPTION 2: **Install [Miniconda](https://docs.conda.io/en/latest/miniconda.html):** \
    Open an Anaconda prompt and create a virtual environment by typing:
    ```
    conda create -n Sports2D python>=3.10 
    conda activate Sports2D
    pip install sports2d
    ```

- OPTION 3: **Build from source and test the last changes:**\
     Open a terminal in the directory of your choice and clone the Sports2D repository.
     ```
     git clone https://github.com/davidpagnon/sports2d.git
     cd sports2d
     pip install .
     ```


### Demonstration: Detect pose and compute 2D angles

Open a terminal, enter `pip show sports2d`, check sports2d package location. \
Copy this path and go to the Demo folder by typing `cd <path>\Sports2D\Demo`. \
Type `ipython`, and test the following code:
```
from Sports2D import Sports2D
Sports2D.detect_pose('Config_demo.toml')
Sports2D.compute_angles('Config_demo.toml')
```

<img src="Content/demo_blazepose_terminal.png" width="760">


You should obtain a video with the overlaid 2D joint positions and angles. This output is also provided as an image folder.\
You should additionally obtain the same information as time series, stored in .csv files.

<img src="Content/demo_blazepose_results.png" width="760">


## Go further

### Use on your own videos

1. Copy-paste `Config_demo.toml` in the directory of your video.

2. Open it with any text editor.\
Replace the `video_file` value with the name of your video.

3. Open a terminal or an Anaconda prompt, type:
   ```
   cd <Path/to/video/directory>
   conda activate Sports2D (skip if you did not install with Anaconda)
   ipython
   ```
   ```
   from Sports2D import Sports2D
   Sports2D.detect_pose('Config_demo.toml')
   Sports2D.compute_angles('Config_demo.toml')
   ```

*Optionally:* If your video is not in the same folder as `Config_demo.toml`, specify its location in `video_dir`.\
Similarly, if you launch Sports2D in an other directory, specify the location of the config file this way: `Sports2D.detect_pose(<path_to_Config_demo.toml>)`\
In `pose`, specify the use of BlazePose or OpenPose as joint detectors: this will be detailed in the next section.\
In `compute_angles`, select your angles of interest.


### Use OpenPose for multi-person, more accurate analysis

1. **Install OpenPose** (instructions [there](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/installation/0_index.md)). \
*Windows portable demo works fine.*

2. If you want even more accurate results, use the BODY_25B experimental model instead of the standard BODY_25 one. This requires manually [downloading the model](https://github.com/CMU-Perceptual-Computing-Lab/openpose_train/blob/master/experimental_models/README.md).

3. In `pose.OPENPOSE`, specify your OpenPose model, and the path where you downloaded OpenPose.

*N.B.:* If you want to benefit from the capabilities of OpenPose but do not manage to install it, you can use the `Colab notebook` version.\
Note that your data will be sent to the Google servers, which do not follow the European GDPR requirements regarding privacy.
**COMING SOON**


### Advanced settings

1. `pose_advanced`: These settings are only taken into account if OpenPose is used.
   1. `load_pose`: If you need to change some settings but have already run a pose estimation, you can set this to `true` in order to not regenerate the json pose files.

   2. `save_vid` and `save_img`: You can choose whether you want to save the resulting video and images. If set to `false`, only pose and angle `.csv` files will be generated.

   3. `interp_gap_smaller_than`: Gaps are interpolated only if they are not too wide.
   
   4. `filter`: `true` or `false`. If `true`, you can choose among `Butterworth`, `Gaussian`, `LOESS`, or `Median`, and specify their parameters.\
   Beware that the appearance of the unfiltered skeleton may not match the filtered coordinates and angles.

   5. `show_plots`: Displays a window with tabs corresponding to the coordinates of each detected point. This may cause Python to crash.

2. `compute_angles_advanced`: These settings are taken into account both with BlazePose and OpenPose.
   1. `save_vid` and `save_img`: Cf `pose_advanced`.

   2. `filter`: Cf `pose_advanced`.

   3. `show_plots`: Cf `pose_advanced`.

*N.B.:* The settings and results of all analyses are stored int the `logs.txt` file, which can be found in in the sports2d installation folder (`pip show sports2d` to find the path).

<img src="Content/demo_show_plots.png" width="760">


### How it works

#### Pose detection:

Detect joint centers from a video with OpenPose or BlazePose.
Save a 2D csv position file per person, and optionally json files, image files, and video files.

If OpenPose is used, multiple persons can be consistently detected across frames.
However, it needs to be [installed](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/installation/0_index.md) separately.
It supports several models: BODY_25 is the standard one, BODY_25B is more accurate but requires manually [downloading the model](https://github.com/CMU-Perceptual-Computing-Lab/openpose_train/blob/master/experimental_models/README.md)
Interpolates sequences of missing data if they are less than N frames long.
Optionally filters results with Butterworth, gaussian, median, or loess filter.
Optionally displays figures.

If BlazePose is used, only one person can be detected.
No interpolation nor filtering options available. Not plotting available.



*N.B.:* Default parameters have been provided in `Demo\Config_demo.toml` but can be edited.\
*N.B.:* OpenPose-like json coordinates are also stored in the `demo_blazepose_json` folder. A `logs.txt` file lets you recover details about your chosen configuration.


#### Angle computation:

Compute joint and segment angles from csv position files.
Automatically adjust angles when person switches to face the other way.
Save a 2D csv angle file per person. These joint and segment angles can be plotted and processed with any spreadsheet software or programming language.
Optionally filters results with Butterworth, gaussian, median, or loess filter.
Optionally displays figures.

Joint angle conventions:
- Ankle dorsiflexion: Between heel and big toe, and ankle and knee
- Knee flexion: Between hip, knee, and ankle 
- Hip flexion: Between knee, hip, and shoulder
- Shoulder flexion: Between hip, shoulder, and elbow
- Elbow flexion: Between wrist, elbow, and shoulder

Segment angle conventions:
Angles are measured anticlockwise between the horizontal and the segment.
- Foot: Between heel and big toe
- Shank: Between ankle and knee
- Thigh: Between hip and knee
- Arm: Between shoulder and elbow
- Forearm: Between elbow and wrist
- Trunk: Between hip midpoint and shoulder midpoint

## How to cite and how to contribute

### How to cite
If you use this code or data, please cite [Pagnon, 2023].

     @misc{Pagnon2023,
       author = {Pagnon, David},
       title = {Sports2D - Angles from monocular video},
       year = {2013},
       publisher = {GitHub},
       journal = {GitHub repository},
       howpublished = {\url{https://github.com/davidpagnon/Sports2D}},
     }

### How to contribute
I would happily welcome any proposal for new features, code improvement, and more!\
If you want to contribute to Sports2D, please follow [this guide](https://docs.github.com/en/get-started/quickstart/contributing-to-projects) on how to fork, modify and push code, and submit a pull request. I would appreciate it if you provided as much useful information as possible about how you modified the code, and a rationale for why you're making this pull request. Please also specify on which operating system and on which python version you have tested the code.

*Here is a to-do list, for general guidance purposes only:*
> <li> <b>GUI applications:</b> For Windows, Mac, and Linux, as well as for Android and iOS (minimal version on mobile device, with only BlazePose). Code with <a href="https://kivy.org">Kivy</a>.</li>
> <li> <b>Pose refinement:</b> Click and move badly estimated 2D points. See <a href="https://www.youtube.com/watch?v=bEuBKB7eqmk">DeepLabCut</a> for inspiration.
> <li> <b>Include OpenPose in Sports2D:</b> For example, <a href="https://github.com/stanfordnmbl/mobile-gaitlab/blob/master/demo/Dockerfile">Dockerize</a> it. Otherwise, run Sports2D in a <a href="https://colab.research.google.com/github/hardik0/AI-basketball-analysis-on-google-colab/blob/master/AI_basketball_analysis_google_colab.ipynb">Colab notebook</a>.
> <li> <b>Constrain points</b> to OpenSim skeletal model for better angle estimation. Cf <a href="https://github.com/perfanalytics/pose2sim">Pose2Sim</a>, but in 2D.
