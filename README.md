# Iso-Dream++: Model-Based Reinforcement Learning with Isolated Imaginations
Code repository for the paper: 

#### Model-Based Reinforcement Learning with Isolated Imaginations 

Minting Pan, Xiangming Zhu, Yunbo Wang, Xiaokang Yang

<!-- If you have any questions, feel free to makes issues. Thanks for your interests! -->


## Description

This work is an extension version of our previous work [(NeurIPS 2022)](https://arxiv.org/abs/2205.13817). For clarity, the detailed contributions of the preliminary conference paper are first summarized as follows:
1.We proposed a novel world model that utilizes modular network structures and inverse dynamics to separate mixed dynamics into controllable and noncontrollable components. This approach enables the model to individually transit different sources of visual dynamics.
2.We introduced a new actor-critic algorithm that makes future-dependent decisions. The action model in this algorithm rolls out noncontrollable dynamics into the future, using an attention mechanism to learn their influence on current behavior. It enables the agent to thoroughly consider possible future interactions with the environment.
3.We evaluated our approach on two reinforcement learning environments, namely the DeepMind Control Suite and CARLA, as well as two real-world datasets for action-conditioned video prediction, namely the BAIR robot pushing and RoboNet.

Summary of changes from the NeurIPS’22 article. This manuscript introduces several key changes to our NeurIPS '22 article, which can be summarized as follows:
1.We propose the min-max variance constraints to isolate different dynamics in an unsupervised manner and prevent information collapse into a single state transition branch (Section 4.1.2). The key idea is to encourage the action-conditioned branch to produce different state transitions based on the same state and distinct actions, while penalizing the diversity of those in the action-free branch.
2.We model the sparse dependency of next-step noncontrollable dynamics on current controllable dynamics to provide a more accurate simulation of some practical dynamic environments (Section 4.1.3). We employ the dependency gate in the action-free branch to determine when the controllable dynamics affect the transition of the next noncontrollable state. 
3.We extend the experiments by including CARLA in the night mode and DeepMind Control Suite with video_hard backgrounds for transfer learning (Section 5.4). The isolation of controllable state transitions further facilitates transfer learning across different but related domains. We can adapt parts of the world model to novel domains based on our prior knowledge of the domain gap.
4.We perform extensive experiments to verify the effectiveness of each proposed component, including ablation studies of the variance constraints (Fig. 14, 15) and the sparse state dependency (Fig. 10, 11).

Overall, these changes significantly improve the effectiveness of our proposed approach in isolating different dynamics, preventing information collapse, modeling sparse dependencies, and facilitating transfer learning across related domains. 
