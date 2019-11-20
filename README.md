### MusiteDeep is a deep-learning based webserver for protein post-translational modification site (PTM) prediction and visualization
The server is available at http://www.musite.net.
This repository contains two independent functions for MusiteDeep webserver:
 >1 a docker compose file for developers locally deploy the MusiteDeep webserver.  
 >2 a stand-alone tool for MusiteDeep webserver

#### 1 Docker deployment:
  For developers who want to deploy MusiteDeep webserver locally.
  - Install docker on your system:
  Only tested on Ubuntu 16.04.5 LST, other Linux system may also work but not guarantee.
  Please refer to https://docs.docker.com/install/linux/docker-ce/ubuntu/ for instructions. 
  - Copy docker compose files:
    copy the file docker-compose.yml to any folder in your system. And in that folder create an empty folder by typing:
    ```sh
    mkdir web
    ```
 - Pull docker:
    ```sh
   sudo docker-compose pull 
    ```
 - Docker up:
   ```sh
   sudo docker-compose up
   ```
#### 2 MusiteDeep stand-alone tool:
In this tool we provide 13 different models for PTM site predictions, users can find the models in the folder of MusiteDeep/models. 
##### Installation

  - Installation has been tested in Linux and Mac OS X with python 3: 
You can install the dependent packages by the following commands:
    ```sh
    apt-get install -y python3 python3-pip
    python3 -m pip install numpy 
    python3 -m pip install scipy
    python3 -m pip install scikit-learn
    python3 -m pip install pillow
    python3 -m pip install h5py
    python3 -m pip install pandas
    python3 -m pip install keras==2.2.4
    python3 -m pip install tensorflow==1.12.0 (or GPU supported tensorflow, pip3 install tensorflow-gpu==1.12.0 refer to https://www.tensorflow.org/install/ for instructions)
    ```
##### Running on GPU or CPU
>If you want to use GPU, you also need to install [CUDA]( https://developer.nvidia.com/cuda-toolkit) and [cuDNN](https://developer.nvidia.com/cudnn); refer to their websites for instructions. 
CPU is only suitable for prediction not training. 

##### For general users who want to perform PTM site prediction by our provided model :
cd to the MusiteDeep folder which contains predict_multi_batch.py
```sh
python3 predict_multi_batch.py -input [custom prediction data in fasta format] -output [custom specified prefix for the prediction results] -model-prefix [prefix of pre-trained model] 
```
The -model-prefix can be "models/XX/", here XX representes one pre-trained model in the folder of "MusiteDeep/models/". Prediction for multiple PTMs, use ";" to seperate them.
For example: 
```sh
python3 predict_multi_batch.py -input testdata/Phosphorylation/Y/test_allspecies_sequences.fasta -output test/output  -model-prefix models/Phosphotyrosine;models/Methyllysine;
```
to predict for phosphotyrosine and methyllysine simultaneously.

##### For advanced users who want to perform training and prediction by using their own data:
```sh
train_CNN_10fold_ensemble -load_average_weight -balance_val -input [custom training data in fasta format] -output [folder for the output models] -checkpointweights [folder for the intermediate checkpoint files] -residue-types [custom specified residue types]

train_capsnet_10fold_ensemble -load_average_weight -balance_val -input [custom training data in fasta format] -output [folder for the output model] -checkpointweights [folder for the intermediate checkpoint files] -residue-types [custom specified residue types]
```

#####Commands used to train our provided models:

```sh
python3 train_CNN_10fold_ensemble.py -load_average_weight -balance_val -input "testdata/Phosphorylation/ST/train_allspecies_sequences_annotated.fasta" -output "./models_test/Phosphoserine_Phosphothreonine/CNNmodels/" -checkpointweights "./models_test/Phosphoserine_Phosphothreonine/CNNmodels/" -residue-types S,T -nclass=1 -maxneg 30

python3 train_capsnet_10fold_ensemble.py -load_average_weight -balance_val -input "testdata/Phosphorylation/ST/train_allspecies_sequences_annotated.fasta" -output "./models_test/Phosphoserine_Phosphothreonine/capsmodels/" -checkpointweights "./models_test/Phosphoserine_Phosphothreonine/capsmodels/" -residue-types S,T -nclass=1 -maxneg 30
```
### Training and testing data are provided in the folder of MusiteDeep/testdata.

### Citation：
Please cite the following paper for using MusiteDeep:
>Wang, D., et al. (2017) MusiteDeep: a deep-learning framework for general and kinase-specific phosphorylation site prediction, Bioinformatics, 33(24), 3909-3916.
>Wang, D., et al. (2019) Capsule network for protein post-translational modification site prediction, Bioinformatics, 35(14), 2386-2394.
