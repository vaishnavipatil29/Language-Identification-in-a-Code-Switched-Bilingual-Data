#  Wav2Vec2 

## Table of contents

<!--ts-->  
   * [Installation and Setup](#installation-and-setup)
   * [Usage](#usage)
      * [For Pretraining](#for-pretraining)
      * [For Finetuning](#for-finetuning)
      * [For Inference](#for-inferences)
      * [For Single File Inference](#for-single-file-inference)
<!--te-->

## Installation and Setup 
### Create an Environment

```
conda create --name <env_name> python=3.7
conda activate <env_name>

sudo apt-get install liblzma-dev libbz2-dev libzstd-dev libsndfile1-dev libopenblas-dev libfftw3-dev libgflags-dev libgoogle-glog-dev
sudo apt install build-essential cmake libboost-system-dev libboost-thread-dev libboost-program-options-dev libboost-test-dev libeigen3-dev zlib1g-dev libbz2-dev liblzma-dev

pip install packaging soundfile swifter
pip install -r requirements.txt
```

### Install fairseq
```
git clone https://github.com/Open-Speech-EkStep/fairseq -b v2-hydra
cd fairseq
pip install -e .
cd ..
```
### Files Setup

1. Create a folder named Wav2vec2. in this folder clone this repository : 
git clone https://github.com/Open-Speech-EkStep/vakyansh-wav2vec2-experimentation/tree/ieee
(Remember we have to clone the ieee branch)
This will create vakyansh-wav2vec2-experimentation folder in the wav2vec2 folder.

OR

You may also use the folder vakyansh-wav2vec2-experimentation in this repository.

2. In the wav2vec2 folder, add your Data folder.
The Data folder should have the structure : 
```
data
|-- raw
|   |-- train
|   |   .wav 
|   |   .txt
|   |-- dev
|   |   .wav
|       .txt
```
# Usage

## For Pretraining

Put the pre-training weights in the vakyansh-wav2vec2-experimentation/checkpoints/pretraining folder.
I have use the CLSRIL - 23 pretrained weights: Cross Lingual Speech Representations for Indic Languages, Contains 10,000 hours of training data from 23 Indic Languages
Link : https://storage.googleapis.com/vakyansh-open-models/pretrained_models/clsril-23/CLSRIL-23.pt
You can download the weights from above link and place it in the vakyansh-wav2vec2-experimentation/checkpoints/pretraining folder.

## For Finetuning

For finetuning, go to vakyansh-wav2vec2-experimentation/scripts/fineuning folder.

1. In this folder, open the prepare_data.sh file and put the appropiate data paths in train_wav_path and dev_wav_path.
To prepapre data : 
```
$ cd scripts/finetuning
$ bash prepare_data.sh
```

2. Check the required paths and values in start_finetuning.sh.
To start finetuning : 

```
$ bash start_finetuning.sh
```

## For Inference
For getting the results, go to vakyansh-wav2vec2-experimentation/scripts/inference folder.

Edit the path to data in the scripts/inference/prepare_data.sh file. To prepare the test data run:
```
$ cd scripts/inference/
$ bash prepare_data.sh
```

Edit the infer.sh file for required paths. To start inference run:
```
$ bash infer.sh
```
After the inferencing is done, 6 files are created in the vakyansh-wav2vec2-experimentation/results/Dev_data folder, from which sentence_wise_wer.csv file gives us the orginal vs predicted labels, along with some other measures. We will be using this file for getting the results metrics.

## For Results Matrix
We will be evaluating our model on these metrics:

1. Accuracy
                  Accuracy =   N / T
Where,
N is the total no. of correctly predicted data samples
T is the total no. of data point

2. Equal Error Rate (EER)

      EER = (FRR+FAR) / 2
      FRR = TFR / T
      FAR = TFA / T
      
where,
EER : Equal Error Rate
FRR : False Rejection Rate
FAR : False Acceptance Rate
TFR : Total No. of False Rejects
TFA : Total No. of False Accepts
T   : Total No. of Datapoints


3. Confusion Matrix

Please Note that the FAR, FRR values and the Confusion Matrix given in the paper, are the combined values for all the L1 languages, i.e values for Telugu, Tamil and Gujarati are added to get the L1 values and all the english and silence values are added to get the L2 and Silence values respectively.

The code for all these metrics are given in the Evaluation folder.

