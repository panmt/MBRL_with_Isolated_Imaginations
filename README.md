# Iso-Dream++: Model-Based Reinforcement Learning with Isolated Imaginations (TPAMI)
Code repository for the paper: 

#### Model-Based Reinforcement Learning with Isolated Imaginations [[arxiv]](https://arxiv.org/abs/2303.14889)

Minting Pan, Xiangming Zhu, Yitao Zheng, Yunbo Wang, Xiaokang Yang

<!-- If you have any questions, feel free to makes issues. Thanks for your interests! -->


## Description

This work is an extension version of our previous work [Iso-Dream (NeurIPS 2022)](https://arxiv.org/abs/2205.13817) [[github]](https://github.com/panmt/Iso-Dream). We propose a novel world model that utilizes modular network structures and inverse dynamics to separate mixed dynamics into controllable and noncontrollable components. This approach enables the model to individually transit different sources of visual dynamics. Furthermore, We introduce a new actor-critic algorithm that makes future-dependent decisions. The action model in this algorithm rolls out noncontrollable dynamics into the future, using an attention mechanism to learn their influence on current behavior. It enables the agent to thoroughly consider possible future interactions with the environment. This work improves Iso-Dream in the following Three aspects:

#### 1. Min-max variance constraints
We propose the min-max variance constraints to isolate different dynamics in an unsupervised manner and prevent information collapse into a single state transition branch. The key idea is to encourage the action-conditioned branch to produce different state transitions based on the same state and distinct actions, while penalizing the diversity of those in the action-free branch.
#### 2. Sparse dependency between decoupled State
We model the sparse dependency of next-step noncontrollable dynamics on current controllable dynamics to provide a more accurate simulation of some practical dynamic environments. We employ the dependency gate in the action-free branch to determine when the controllable dynamics affect the transition of the next noncontrollable state. 
#### 3. Transfer learning
We extend the experiments by including CARLA in the night mode and DeepMind Control Suite with video_hard backgrounds for transfer learning. The isolation of controllable state transitions further facilitates transfer learning across different but related domains. We can adapt parts of the world model to novel domains based on our prior knowledge of the domain gap.

## Get Started
Iso-Dream is implemented and tested on Ubuntu 18.04 with python == 3.7, PyTorch == 1.9.0:

1. Create an environment 
   ```
   conda create -n iso-env python=3.7
   conda activate iso-env
   ```   

2. Install dependencies
   ```
   pip install -r requirements.txt
   ```

### CARLA / DMC

#### For CARLA environment:

  1. Setup
  
     Download and setup CARLA 0.9.10
     ```
     cd iso_rl
     chmod +x setup_carla.sh
     ./setup_carla.sh
     ```
     
     Add to your python path:
     ```
     export PYTHONPATH=$PYTHONPATH:/home/CARLA_0.9.10/PythonAPI
     export PYTHONPATH=$PYTHONPATH:/home/CARLA_0.9.10/PythonAPI/carla
     export PYTHONPATH=$PYTHONPATH:/home/CARLA_0.9.10/PythonAPI/carla/dist/carla-0.9.10-py3.7-linux-x86_64.egg
     ```
     and merge the directories.

  2. Training
  
     Terminal 1:
     ```
     cd CARLA_0.9.10
     bash CarlaUE4.sh -fps 20 -opengl
     ```

     Terminal 2:
     ```
     cd iso_rl
     python dreamer.py --logdir log/iso_carla --sz_sparse True --min_free True --max_action True --seed 9 --configs defaults carla
     ```

  3. Evaluation
     ```
     cd iso_rl
     python test.py --logdir test --sz_sparse True --min_free True --max_action True --configs defaults carla
     ```

#### For DMC environment:
  1. Setup DMC with video background
  
     Download 'envs' from [Google Drive](https://drive.google.com/drive/folders/1vAHRBx7zlK-XHowSOAv-gBPWlubvpnCo?usp=sharing) and put it in the 'iso_rl'. The dependencies can then be installed with the following commands:
  
     ```
     cd iso_rl
     
     cd ./envs/dm_control
     pip install -e .
     
     cd ../dmc2gym
     pip install -e .

     cd ../..
     ```

  2. Training
     ```
     python dreamer.py --logdir log/iso_dmc --sz_sparse False --min_free True --max_action True --seed 4 --configs defaults dmc --task dmcbg_walker_walk
     ```
  

### BAIR / RoboNet
Train and test Iso-Dream on BAIR and RoboNet datasets. Also, install Tensorflow 2.1.0 for BAIR dataloader.

1. Download BAIR data. 
   ```
   wget http://rail.eecs.berkeley.edu/datasets/bair_robot_pushing_dataset_v0.tar
   ```

2. Train the model. You can use the following bash script to train the model. The learned model will be saved in the `--save_dir` folder.
  The generated future frames will be saved in the `--gen_frm_dir` folder. 
    ```
    cd iso_video_prediction
    sh train_iso_model.sh
    ```


## Acknowledgement
We appreciate the following github repos where we borrow code from:

https://github.com/jsikyoon/dreamer-torch

https://github.com/thuml/predrnn-pytorch
