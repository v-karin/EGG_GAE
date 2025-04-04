# EdGe Generation-Graph AutoEncoder (EGG-GAE)

(AMENDED REPO)

---

[EGG-GAE: scalable graph neural networks for tabular data imputation](https://arxiv.org/abs/2210.10446)
# Navigation
- [Repository structure](#repository_structure)
- [Setup enviroment](#setup_environment)
- [Run an experiment](#run_an_experiment)

## Setup environment
Higher versions of torch/pytorch_geometric may be compatible.
This repo has been amended to run with the following environment below.

Run the commands below, setting up a conda environment:

```
conda create --name egg_exp --yes
conda activate egg_exp
pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0
pip install pytorch-lightning==2.5.1
pip install torch_geometric==2.6.1
pip install pandas==2.2.3
pip install scikit-learn==1.6.1

pip install hydra-core==1.3.2
conda install ipykernel ipywidgets networkx
```

~~#pip install wandb==0.13.9~~

~~#conda install -c conda-forge graph-tool gudhi --yes~~

## Configs
Please refer to the `configs/defaults.yaml` file for more details on the available configuration options.

## Run an experiment
The code is designed to be executed on a single GPU and the GPU must be indicated through the CLI:

```
CUDA_VISIBLE_DEVICES=<GPU_ID> python run.py
```

## Adding Tabular Dataset

To add a tabular dataset, modify `data_prep/upload.py` and update the `loader` and `upload_data` function. 
The output of the `loader` function is:
- `X`: observation features
- `y`: observation labels 

In the `upload_data` function, it is necessary to add a dictionary with the parameters of the tabular dataset (see the code for examples)


## Logger
~~Login to your `wandb` account, running once `wandb login`.~~
~~Configure the logging in `conf/logging/*`.~~

~~Read more in the [docs](https://docs.wandb.ai/). Particularly useful the [`log` method](https://docs.wandb.ai/library/log),~~ accessible from inside a PyTorch Lightning module with `self.logger.experiment.log`.

> Since we are using Lightning, you can replace `wandb` with the logger you prefer (you can even build your own).
 More about Lightning loggers [here](https://pytorch-lightning.readthedocs.io/en/latest/extensions/logging.html).

## Additional Configs
To understand the structure see [hydra](https://hydra.cc/docs/tutorials/basic/your_first_app/config_groups/).
dataset.yaml and model.yaml consist of dataset_type and model_type keys respectively. Through keys values pl pipline is configured.

Configure all parameters through .yaml file.

## Repository structure
```

│   README.md
│   method.py
|   dataloader.py
|   train.py
|   utils.py
|
└───configs
│   │   defaults.yaml
│   └─── dataloader
│   │    │  dataset.yaml
│   │
│   └─── model
│   │    │  egg_model.yaml
│   │    │  kegg_model.yaml
|
└───models
│   │   modules 
│   │   │   egg.py
│   │   │   mappers.py
│   │   networks
│   │   │   EGnet.py
│   │   edge_generation
│   │   │   EGmodule.py
│   │   │   distances.py
│   │   │   sampler.py
│   │   model.py
|
└───datasets
│   │   dataset.py
│   │   upload_data.py
|
└───data_prep
│   │   save.py
│   │   upload.py
│   │   pipeline.py
│   │   preprocesing.py
│   │   utils.py
│   │   miss_utils.py
|
└───inference
│   │   ensembler.py
│   │   val_logic.py
│   │   test_logic.py
│   │   utils.py
|

```

### How do I use this code ###
The core of this repository is that the pytorch-lightning (pl) pipline is configured though .yaml file.
There are few key points of this repository:
- write your data preprocessing pipline in dataset file (see the **toy** dataset.py and transformfactory.py)
- write your model and pl logic in model file (see the **toy** model.py)
- configure your pipline through .yaml file and see all metrics in [wadb](https://docs.wandb.ai/)



