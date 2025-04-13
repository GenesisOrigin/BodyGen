# BodyGen: Advancing Towards Efficient Embodiment Co-Design

<p align="center">
·
<a href="https://openreview.net/pdf?id=cTR17xl89h">Paper</a>
·
<a href="https://github.com/GenesisOrigin/BodyGen">Code</a>
·
<a href="https://genesisorigin.github.io">Website</a>
·
<a href="https://huggingface.co/Josh00/BodyGen">Hugging Face</a>
</p>

This repository contains the PyTorch implementation of *"BodyGen: Advancing Towards Efficient Embodiment Co-Design."* (ICLR 2025, Spotlight). Here is our [poster](https://x.com/josh00_lu/status/1911402125818548569). Glad to chat & connect in Singapore. ☺️

<p align="center">
    <br>
    <img src="figures/framework.png"/>
    <br>
<p>

## 🛠️ Setup
Let's start with python 3.9. It's recommend to create a `conda` env:

### Create a new conda environment 
```bash
conda create -n BodyGen python=3.9
conda activate BodyGen
```

### Install for MuJoCo Simulator and mujoco-py (Important)
Install mujoco-py following the instruction [here](https://github.com/openai/mujoco-py#install-mujoco).

Set the following environment variable to avoid problems with multiprocess trajectory sampling:
```bash
export OMP_NUM_THREADS=1
```

(Optional) For MacOS user, you can follow the `README_FOR_MAC.md` to install MuJoCo on M1/M2/M3 Mac, which is helpful for embodied agents visualization.

### Install dependencies
```bash
pip install -r requirements.txt
```

Note you may have to follow https://pytorch.org/ setup instructions for installation on your own machine.

## 👀 Visualization

<p align="center">
    <br>
    <img src="figures/visualization.png"/>
    <br>
<p>

Please refer to this [website](https://genesisorigin.github.io) for more visualization results. If you want to visualize the pretrained models, please refer to the following section.

### (Optional) Pretrained Models for Playing
We also provide pretrained models in `BodyGen/pretrained_models` for visualization. 

* You can download pretrained models from [Google Drive](https://drive.google.com/file/d/1TYRl8FI8TWEkXr1wYGOsW0au--GUBnce/view?usp=sharing)

* Once the `pretrained_models.zip` file is downloaded, unzip it under the folder `BodyGen` of this repo:
```bash
unzip pretrained_models.zip
```

After you unzip the file, an example directory hierarchy is:
```bash
assets/
design_opt/
...
pretrained_models/
  |-- cheetah/
  |-- crawler/
  |-- ...
  |-- walker-regular/
...
scripts/
```
Our pretrained models are also available on [Hugging Face](https://huggingface.co/Josh00/BodyGen). You can download the pretrained models, and place the `pretrained_models` folder under the root directory (./BodyGen) of this repo.

### Interactive Visualization Locally

If you have a GUI display, you can run the following command to visualize the pretrained model:
```bash
python design_opt/eval.py --train_dir <path_of_model_folder>
```

For example, you can use `pretrained_models` to visualize the co-design embodied agent on `cheetah` generated by BodyGen:
```bash
python design_opt/eval.py --train_dir /Path/to/BodyGen/pretrained_models/cheetah
```

Press `S` to slow the agent, and `F` to speed up. 

## 💻 Training
```bash
cd BodyGen
chmod 777 scripts/Run_BodyGen.sh
./scripts/Run_BodyGen.sh
```
Use the scripts `scripts/Run_BodyGen.sh`, which contain preconfigured arguments and hyperparameters for **all** the experiments in the paper.  Experiments use Hydra to manage configuration.

Visualizing results requires `wandb`: configure project name with the `project` key in `BodyGen/design_opt/conf/config.yaml`.

As an example of how to run, this runs BodyGen on the `crawler` environment:

```bash
EXPT="crawler"
OMP_NUM_THREADS=1 python -m design_opt.train -m cfg=$EXPT group=$EXPT
```

Replace `crawler` with {`crawler`, `terraincrosser`, `cheetah`, `swimmer`, `glider-regular`, `glider-medium`, `glider-hard`, `walker-regular`, `walker-medium`, `walker-hard`} to train other environments.

- `OMP_NUM_THREADS=1` is essential to prevent CPU ops from hanging.

- The environment is selected with the `cfg=` flag, each of which corresponds to a YAML file in `BodyGen/design_opt/cfg`. See that folder
for the list of available experiments.

- Other hyperparameters are explained in `BodyGen/design_opt/conf/config.yaml` and our paper.

## 🙏 Acknowledgement
* Our initial code is based on [Transform2Act](https://github.com/Khrylx/Transform2Act). Thanks for their great work and the discussions with the authors.
* We also refer to [Neural Graph Evolution (NGE)](https://github.com/WilsonWangTHU/neural_graph_evolution) and [One-Policy-to-Control-Them-All](https://github.com/huangwl18/modular-rl) during our implementation. Thanks for their interesting work.
* The initial designs are based on [Transformer2Act](https://github.com/Khrylx/Transform2Act), [OpenAI Gym](https://github.com/openai/gym), and [DeepMind dm_control](https://github.com/google-deepmind/dm_control). All the algorithms are evaluated with the same sets of initial designs.
* The backend engine is based on the [MuJoCo](https://github.com/google-deepmind/mujoco), and we are planning to bump to the latest version of official MuJoCo bindings (>3.0.1) in the future.

## 📚 Citation
If you find our work useful in your research, please consider citing:
```bibtex
@inproceedings{
  lu2025bodygen,
  title={BodyGen: Advancing Towards Efficient Embodiment Co-Design},
  author={Haofei Lu and Zhe Wu and Junliang Xing and Jianshu Li and Ruoyu Li and Zhe Li and Yuanchun Shi},
  booktitle={The Thirteenth International Conference on Learning Representations},
  year={2025},
  url={https://openreview.net/forum?id=cTR17xl89h}
}
```

## 🏷️ License
Please see the [license](LICENSE) for further details.
