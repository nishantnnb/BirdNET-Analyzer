[![CC BY-NC-SA 4.0][license-badge]][cc-by-nc-sa] 
![Supported OS][os-badge]
![Number of species][species-badge]

[license-badge]: https://badgen.net/badge/License/CC-BY-NC-SA%204.0/green
[os-badge]: https://badgen.net/badge/OS/Linux%2C%20Windows/blue
[species-badge]: https://badgen.net/badge/Species/1318/blue

# BirdNET-Analyzer
BirdNET analyzer script for processing large amounts of audio data or single audio files. This is the most advanced version of BirdNET for acoustic analyses and we will keep this repository up-to-date with new models and improved interfaces to enable scientists with no CS background to run the analysis.

Feel free to use BirdNET for your acoustic analyses and research. If you do, please cite as:

```
@article{kahl2021birdnet,
  title={BirdNET: A deep learning solution for avian diversity monitoring},
  author={Kahl, Stefan and Wood, Connor M and Eibl, Maximilian and Klinck, Holger},
  journal={Ecological Informatics},
  volume={61},
  pages={101236},
  year={2021},
  publisher={Elsevier}
}
```

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/

## About

Developed by the [K. Lisa Yang Center for Conservation Bioacoustics](https://www.birds.cornell.edu/ccb/), [Cornell Lab of Ornithology](https://www.birds.cornell.edu/home), [Cornell University](https://www.cornell.edu)

Go to https://birdnet.cornell.edu to learn more about the project.

Follow us on Twitter [@BirdNET_App](https://twitter.com/BirdNET_App).

Want to use BirdNET to analyze a large dataset? Don't hesitate to contact us: ccb-birdnet@cornell.edu

<b>Have a question, remark or feature request? Please start a new issue thread to let us know. Feel free to submit pull request.</b>

## Model version update

**V2.0 BETA**

- same model design as 1.4 but a bit wider
- extended 2022 training data
- global selection of species (birds and non-birds) with 1,328 classes (incl. 10 non-event classes)

**V1.4**

- smaller, deeper, faster
- only 30% of the size of V1.3
- still linear spectrogram and EfficientNet blocks
- extended 2021 training data
- 1,133 birds and non-birds for North America and Europe

**V1.3**

- Model uses linear frequency scale for spectrograms
- uses V2 fusion blocks and V1 efficient blocks
- extended 2021 training data
- 1,133 birds and non-birds for North America and Europe

**V1.2**

- Model based on EfficientNet V2 blocks
- uses V2 fusion blocks and V1 efficient blocks
- extended 2021 training data
- 1,133 birds and non-birds for North America and Europe

**V1.1**

- Model based on Wide-ResNet (aka "App model")
- extended 2021 training data
- 1,133 birds and non-birds for North America and Europe

**App Model**

- Model based on Wide-ResNet
- ~3,000 species worldwide
- currently deployed as BirdNET app model

## Setup (Ubuntu)

Install Python 3:
```
sudo apt-get update
sudo apt-get install python3-dev python3-pip
sudo pip3 install --upgrade pip
```

Install Tensorflow (has to be 2.5 or later):
```
sudo pip3 install tensorflow
```

Install Librosa to handle audio files:

```
sudo pip3 install librosa
sudo apt-get install ffmpeg
```

Clone the repository

```
git clone https://github.com/kahst/BirdNET-Analyzer.git
cd BirdNET-Analyzer
```
## Setup (Windows)

Install Python 3.8 (has to be 64bit version)

- Download and run installer: [Download Python installer](https://www.python.org/ftp/python/3.8.0/python-3.8.0-amd64.exe)
- :exclamation:<b>Make sure to check "Add path to environment variables" during install</b>:exclamation:

Install Tensorflow (has to be 2.5 or later), Librosa and NumPy

- Open command prompt with "Win+S" type "command" and click on "Command Prompt"
- Type `pip install --upgrade pip`
- Type `pip install tensorflow librosa numpy==1.20`

<b>NOTE</b>: You might need to run the command prompt as "administrator". Type "Win+S", search for command prompt and then right-click, select "Run as administrator".

Install Visual Studio Code (optional)

- Download and install VS Code: [Download VS Code installer](https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-user)
- Select all available options during install

Install BirdNET using Git (for simple download see below)

- Download and install Git Bash: [Download Git Bash installer](https://github.com/git-for-windows/git/releases/download/v2.34.1.windows.1/Git-2.34.1-64-bit.exe)
- Select Visual Studio Code as default editor (optional)
- Keep all other settings as recommended
- Create folder in personal directory called "Code" (or similar)
- Change to folder and right click, launch "Git bash here"
- Type `git clone https://github.com/kahst/BirdNET-Analyzer.git`
- Keep BirdNET updated by running `git pull` for BirdNET-Analyzer folder occasionally

Install BirdNET from zip

- Download BirdNET: [Download BirdNET Zip-file](https://github.com/kahst/BirdNET-Analyzer/archive/refs/heads/main.zip)
- Unpack zip file (e.g., in personal directory)
- Keep BirdNET updated by re-downloading the zip file occasionally and overwrite existing files

Run BirdNET from command line

- Open command prompt with "Win+S" type "command" and click on "Command Prompt"
- Navigate to the folder where you installed BirdNET (cd path\to\BirdNET-Analyzer)
- See "Usage" section for command line arguments

<b>NOTE</b>: With Visual Studio Code installed, you can right-click the BirdNET-Analyzer folder and select "Open with Code". With proper extensions installed (View --> Extensions --> Python) you will be able to run all scripts from within VS Code.

## Usage

1. Inspect config file for options and settings, especially inference settings. Specify a custom species list if needed and adjust the number of threads TFLite can use to run the inference.

2. Run `analyzer.py` to analyze an audio file. You need to set paths for the audio file and selection table output. Here is an example:

```
python3 analyze.py --i /path/to/audio/folder --o /path/to/output/folder
```

<b>NOTE</b>: Your custom species list has to be named 'species_list.txt' and the folder containing the list needs to be specified with `--slist /path/to/folder`. You can also specify the number of CPU threads that should be used for the analysis with `--threads <Integer>` (e.g., `--threads 16`). If you provide GPS coordinates with `--lat` and `--lon`, the custom species list argument will be ignored.

Here's a complete list of all command line arguments:

```
--i, Path to input file or folder. If this is a file, --o needs to be a file too.
--o, Path to output file or folder. If this is a file, --i needs to be a file too.
--lat, Recording location latitude. Set -1 to ignore.
--lon, Recording location longitude. Set -1 to ignore.
--week, Week of the year when the recording was made. Values in [1, 48] (4 weeks per month). Set -1 for year-round species list.
--slist, Path to species list file or folder. If folder is provided, species list needs to be named "species_list.txt". If lat and lon are provided, this list will be ignored.
--sensitivity, Detection sensitivity; Higher values result in higher sensitivity. Values in [0.5, 1.5]. Defaults to 1.0.
--min_conf, Minimum confidence threshold. Values in [0.01, 0.99]. Defaults to 0.1.
--overlap, Overlap of prediction segments. Values in [0.0, 2.9]. Defaults to 0.0.
--rtype, Specifies output format. Values in ['table', 'csv']. Defaults to 'table' (Raven selection table).
--threads, Number of CPU threads.
--batchsize, Number of samples to process at the same time. Defaults to 1.
```

Here are two example commands to run this BirdNET version:

```
python3 analyze.py --i example/ --o example/ --slist example/ --min_conf 0.5 --threads 4

python3 analyze.py --i example/ --o example/ --lat 42.5 --lon -76.45 --week 4 --sensitivity 1.0
```

3. Run `embeddings.py` to extract feature embeddings instead of class predictions. Result file will contain timestamps and lists of float values representing the embedding for a particular 3-second segment. Embeddings can be used for clustering or similarity analysis. Here is an example:

```
python3 embeddings.py --i example/ --o example/ --threads 4 --batchsize 16
```

Here's a complete list of all command line arguments:

```
--i, Path to input file or folder. If this is a file, --o needs to be a file too.
--o, Path to output file or folder. If this is a file, --i needs to be a file too.
--threads, Number of CPU threads.
--batchsize, Number of samples to process at the same time. Defaults to 1.
```

4. When editing your own `species_list.txt` file, make sure to copy species names from the labels file of each model. 

You can find label files in the checkpoints folder, e.g., `checkpoints/V2.0/BirdNET_GLOBAL_1K_V2.0_Labels.txt`. 

Species names need to consist of `scientific name_common name` to be valid.

5. You can generate a species list for a given location using `species.py` in case you need it for reference. Here is an example:

```
python3 species.py --o example/species_list.txt --lat 42.5 --lon -76.45 --week 4
```

Here's a complete list of all command line arguments:

```
--o, Path to output file or folder. If this is a folder, file will be named 'species_list.txt'.
--lat, Recording location latitude. Set -1 to ignore.
--lon, Recording location longitude. Set -1 to ignore.
--week, Week of the year when the recording was made. Values in [1, 48] (4 weeks per month). Set -1 for year-round species list.
--threshold, Occurrence frequency threshold. Defaults to 0.05.
--sortby, Sort species by occurrence frequency or alphabetically. Values in ['freq', 'alpha']. Defaults to 'freq'.
```

6. This is a very basic version of the analysis workflow, you might need to adjust it to your own needs.

7. Please open an issue to ask for new features or to document unexpected behavior.

8. I will keep models up to date and upload new checkpoints whenever there is an improvement in performance. I will also provide quantized and pruned model files for distribution.

## Usage (Docker)

Install docker for Ubuntu:

```
sudo apt install docker.io
```

Build Docker container:

```
sudo docker build -t birdnet .
```

<b>NOTE</b>: You need to run docker build again whenever you make changes to the script.

In order to pass a directory that contains your audio files to the docker file, you need to mount it inside the docker container with <i>-v /my/path:/mount/path</i> before you can run the container. 

You can run the container for the provided example soundscapes with:

```
sudo docker run -v $PWD/example:/audio birdnet --i audio --o audio --slist audio
```

You can adjust the directory that contains your recordings by providing an absolute path:

```
sudo docker run -v /path/to/your/audio/files:/audio birdnet --i audio --o audio --slist audio
```

You can also mount more than one drive, e.g., if input and output folder should be different:

```
sudo docker run -v /path/to/your/audio/files:/input -v /path/to/your/output/folder:/output birdnet --i input --o output --slist input
```

See "Usage" section above for more command line arguments, all of them will work with Docker version.

<b>NOTE</b>: If you like to specify a species list (which will be used as post-filter and needs to be named 'species_list.txt'), you need to put it into a folder that also has to be mounted. 

## Funding

This project is supported by Jake Holshuh (Cornell class of ’69). The Arthur Vining Davis Foundations also kindly support our efforts.
