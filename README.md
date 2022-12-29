# AMZ ROSBag Filter v1.0
### What is it?
This tool allows for simple and efficient search for potentially useful _ROSBags_. The idea is: whenever you are looking for a specific _ROSBag_, you can tell the tool what you need and it will output **paths** to _ROSBags_ which **could** be potentially interesting for you. This spares you the effort of investigating every single _ROSBag_ manually with `rosbag info`.
### Installation
The tool is targeted to use on the **NAS**, where it has already been installed and is ready to use (have a look at `Usage` section of this README), but you could theoretically also deploy it on your local machine. For this you need to:
1. Clone the repository
2. Make sure that you are using Python version which is `>= 3.8`
3. Install dependencies. You can do this on **Ubuntu** and **Debian** distributions by running the command below:
    ```
    sudo apt update
    pip3 install --upgrade pip3
    pip3 install pandas argparse termcolor rospy_message_converter rosbags
    ```

### Usage
This version (v1.0) consists of **two** Python scripts:
1. `analyzer.py` - used for generating CSV files which contain the properties of different _ROSBags_,
2. `filter.py` - takes as input the parameters desired by the user and searches through the CSV files generated by the analyzer to find _ROSBags_, which satisfy the demanded properties.

In order to run them, pass the **Python3** interpreter the path to the script with additional **flags/options**.
For `analyzer.py` these include:
```
  -h, --help  show this help message and exit
  -d D        Not optional - take an absolute path to a directory and analyses all ROSBags within it
  -r          The directory passed with -d flag is searched recursively (entire file tree starting at given path is
              analysed). WARNING - can be very slow!
```
And for `filter.py`:
```
  -h, --help   show this help message and exit
  --per        Filter for rosbags where perception runs at ~10 Hz
  --est        Filter for rosbags where estimation runs at ~10 Hz
  --con        Filter for rosgags where control runs at ~10 Hz
  --vel VEL    Filter for rosbags where avg. car velocity (in m/s) is at least VEL (default: 1)
  --laps LAPS  Filter for rosbags where car does at least LAPS laps (default: 0)
  --dur DUR    Filter for rosbags which are at least DUR seconds long (default: 60)
  -d D         Not optional - take an absolute path to a directory and analyses all ROSBags within it
  -r           The directory with rosbags is filtered recursively (entire file tree starting at given path is
               filtered).
```

Examples:
```
# Analyse the directory /home/amz-nas/pilatus-2022/2022-07-29_IPZ/rosbags-small to create the rosbag_analysis.csv file used by filter
python3 /path/to/analyzer_script/analyzer.py -d /home/amz-nas/pilatus-2022/2022-07-29_IPZ/rosbags-small

# Recursively filter the directory /home/amz-nas/bernina-2022/2022-08-03_IPZ for ROSBags where perception and control run at ~10 Hz, avg. velocity is equal to at least 2m/s and the duration is at least 90 seconds.
python3 /path/to/filter_script/filter.py -d /home/amz-nas/bernina-2022/2022-08-03_IPZ -r --per --con --vel 2 --dur 90

# Filter the directory /home/amz-nas/pilatus-2022/2022-06-08_IPZ/rosbags for ROSBags where estimation runs at ~10 Hz and the car does at least 2 laps.
python3 /path/to/filter_script/filter.py -d /home/amz-nas/pilatus-2022/2022-06-08_IPZ/rosbags --est --laps 2
```

### Additional functionality
If there are any properties that you would like to filter the _ROSBags_ after, that you (and maybe also the rest of the team) could make use of, please create an issue in this repository! I am open for any suggestions!


Maintainer: Stanislaw Piasecki