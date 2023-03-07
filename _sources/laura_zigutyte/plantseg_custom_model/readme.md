# Training Custom Plantseg Model (Pytorch)

This blog post aims to help biologists that are not very experienced in programming and deep learning to train their own model for [PlantSeg](https://github.com/hci-unihd/plant-seg). PlantSeg is ...
Have you already tried to predict with provided pre-trained models, and felt like results were good but not good enough for your data because your data differs substantially from plant data the models were trained on? Have you thought about training your own model, however, feel intimidated by the instructions in the [*pytorch-3dunet* repository on GitHub](https://github.com/wolny/pytorch-3dunet) because originally the training is done from the terminal with configuration files? Then this is a post for you! This blog post tries to simplify the process of training a deep learning model by providing instructions on **how to do it from the jupyter notebook**, which is a more user-friendly way. You can also use these instructions to **tune the pre-trained model** with your data instead of performing the training from scratch.

## What will you need?
* A **conda/mamba environment** or a **singularity container** with required packages (described in Step 1). If you don't have yet conda/mamba installed on your machine, you can read [this blog post by Mara](../../mara_lampert/getting_started_with_mambaforge_and_python/readme) on how to set it up. Or if you want to do the training on the HPC cluster you can read [this blog post by Till](../../till_korten/???) on how to get started with it.
* **Annotated (labelled) data**, where labels are of good quality, otherwise, the model will learn bad "knowledge".
* Quite a bit of **data** if you are training from scratch because deep learning models are **data greedy**. Therefore, you need at least a couple of hundred of 3D timelapse time points. The model needs to "see" a variety of images during training to "learn" well, that means to perform later on well on "unseen" data.
* **Hardware powerful** enough for deep learning, including NVIDIA GPU. This will also depend on your data and parameters that you choose for the training, particularly patch size, which determines how big overlapping patches will be extracted from your images. One patch should fit into your GPU memory but it should not be way smaller than objects in your images. Therefore, it might be that a laptop with a GPU of 4 or 8 GB will be enough for the training but it also can happen that you will need to do the training on the workstation or HPC cluster. 

> _Note:_ The training in this blog post example was done on the Taurus cluster at TU Dresden with 4 NVIDIA A100 GPUs.

## Custom training Plantseg model is not for you if:
* You do not have good quality annotated data (so-called ground truth data)
* You have very little data
* You don't have access to the hardware needed to handle your data and deep learning model's training

## Step 1: Prepare the Environment/container

If you are using conda/mamba to create the environment, the easiest way to install required packages is via the following command:

```
mamba create -n plantseg-env -c pytorch -c conda-forge -c lcerrone -c awolny plantseg

conda activate pytorch3dunet
```

Pay attention that this command will install the newest available versions of libraries (or pinned versions in some cases). Depending on your GPU, you might need a different version of cuda for pytorch to be able to run on the GPU and not only CPU, which is very important for speed of the training. You can check which versions you would need [here](https://pytorch.org/get-started/locally/), and follow installation instructions accordingly. 

> _Note:_ If you want to train the model on the alpha partition of the Taurus cluster at TUD, the versions that you need to install can be found [here](environment.txt).

To check whether you have all packages that we will need installed, run the following cell and see whether it executes without any errors.



