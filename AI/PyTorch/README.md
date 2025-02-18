# PyTorch

<img src="Images/pytorch-logo.png" alt="PyTorch-logo" width="200"/>

## What is PyTorch?

[PyTorch](https://pytorch.org) is a GPU accelerated tensor computational framework with a Python front end. Functionality can be easily extended with common Python libraries such as NumPy, SciPy, and Cython. Automatic differentiation is done with a tape-based system at both a functional and neural network layer level. This functionality brings a high level of flexibility and speed as a deep learning framework and provides accelerated NumPy-like functionality.

## Installing PyTorch

These instructions are intended to help you install PyTorch on the FASRC cluster.

### GPU Support

For general information on running GPU jobs refer to our [user documentation](https://www.rc.fas.harvard.edu/resources/documentation/gpgpu-computing-on-the-cluster).

To set up PyTorch with GPU support in your user environment, please follow the below steps:

(1) Start an interactive job requesting GPUs, e.g., (Note: you will want to start a session on the same type of hardware as what you will run on)

```bash
salloc -p gpu -t 0-06:00 --mem=8000 --gres=gpu:1 
```

(2) Load required software modules, e.g.,

```bash
module load python/3.10.13-fasrc01
```

(3) Create a [conda environment](https://conda.io/projects/conda/en/latest/index.html), e.g.,

```bash
mamba create -n pt2.2.1_cuda12.1 python=3.10 pip wheel
```

(4) Activate the new `conda` environment:

```bash
source activate pt2.2.1_cuda12.1
```

(5) Install `cuda-toolkit` version 12.1.0 with `mamba`

```bash
mamba install -c "nvidia/label/cuda-12.1.0" cuda-toolkit
```

(6) Install PyTorch with `mamba`

```bash
mamba install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

(7) Install additional Python packages, if needed, e.g.,

```bash
mamba install -c conda-forge numpy scipy pandas matplotlib seaborn h5py jupyterlab jupyterlab-spellchecker scikit-learn
```

### Other PyTorch/cuda versions

To install other versions, refer to the PyTorch [compatibility chart](https://pytorch.org/):

<img src="Images/pytorch-chart.png" alt="pytorch-chart" width="80%"/>

## Running PyTorch:

### Run PyTorch Interactively

For an **interactive session** to work with the GPUs you can use following:

```bash
salloc -p gpu -t 0-06:00 --mem=8000 --gres=gpu:1 
```

Load required software modules and source your PyTorch conda environment.

```bash
[username@holygpu7c26103 ~]$ module load python/3.10.12-fasrc01
[username@holygpu7c26103 ~]$ source activate pt2.1.0_cuda12.1
(pt2.1.0_cuda12.1) [username@holygpu7c26103 ~]$
```

Test PyTorch interactively:

```bash
(pt2.1.0_cuda12.1) [username@holygpu7c26103 ~]$ python check_gpu.py
Using device: cuda

NVIDIA A100-SXM4-40GB
Memory Usage:
Allocated: 0.0 GB
Reserved:  0.0 GB

tensor([[-2.3792, -1.2330, -0.5143,  0.5844]], device='cuda:0')
```

The below code, <code>check_gpu.py</code>, checks if GPUs are available and if available sets up the device to use them.

```python
#!/usr/bin/env python
import torch

# Setting device on GPU if available, else CPU
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
print('Using device:', device)
print()

# Print out additional information when using CUDA
if device.type == 'cuda':
    print(torch.cuda.get_device_name(0))
    print('Memory Usage:')
    print('Allocated:', round(torch.cuda.memory_allocated(0)/1024**3,1), 'GB')
    print('Reserved: ', round(torch.cuda.memory_reserved(0)/1024**3,1), 'GB')
    print()

# Run a small test on the available device
T = torch.randn(1, 4).to(device)
print(T)
```

### Run PyTorch with Batch Jobs

An example batch-job submission script is included below:

```bash
#!/bin/bash
#SBATCH -c 1
#SBATCH -N 1
#SBATCH -t 0-00:30
#SBATCH -p gpu
#SBATCH --gres=gpu:1
#SBATCH --mem=4G
#SBATCH -o pytorch_%j.out 
#SBATCH -e pytorch_%j.err 

# Load software modules and source conda environment
module load python/3.10.12-fasrc01
source activate pt2.1.0_cuda12.1

# Run program
srun -c 1 --gres=gpu:1 python check_gpu.py 
```

If you name the above batch-job submission script <code>run.sbatch</code>, for instance, the job is submitted with:

```bash
sbatch run.sbatch
```

## Installing PyG (torch geometry)

After you create the conda environment `pt2.1.0_cuda12.1` and activated it, you can install [PyG](https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html)
in your environment with the command:

```bash
(pt2.1.0_cuda12.1) [username@holygpu7c26103 ~]$ mamba install pyg -c pyg
```

## PyTorch and Jupyter Notebook on Open OnDemand

If you would like to use the PyTorch environment on [Open OnDemand/VDI](https://vdi.rc.fas.harvard.edu/), you will also need to install packages `ipykernel` and `ipywidgets` with the following commands:

```bash
(pt2.1.0_cuda12.1) [username@holygpu7c26103 ~]$ mamba install ipykernel ipywidgets
```

## Pull a PyTorch Singularity container

Alternatively, one can pull and use a PyTorch [singularity](https://docs.sylabs.io/guides/3.5/user-guide/index.html) container:

```bash
singularity pull docker://pytorch/pytorch:2.1.0-cuda12.1-cudnn8-runtime
```
The specific example illustrates this for `PyTorch` version `2.1.0` with GPU support with `CUDA` version `12.1`. This will result in the image <code>pytorch_2.1.0-cuda12.1-cudnn8-runtime.sif</code>. The image then can be used with, e.g.,

```python
$ singularity exec --nv pytorch_2.1.0-cuda12.1-cudnn8-runtime.sif python
Python 3.10.13 (main, Sep 11 2023, 13:44:35) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> print(torch.__version__)
2.1.0
>>> device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
>>> print('Using device:', device)
Using device: cuda
>>> T = torch.randn(1, 4).to(device)
>>> print(T)
tensor([[1.2458, 0.9938, 0.4733, 0.3014]], device='cuda:0')
```

Alternatively, you can also pull a PyTorch singularity image from the [NVIDIA NGC Catalog](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch), e.g.:

```bash
singularity pull docker://nvcr.io/nvidia/pytorch:23.09-py3
```
This will result in the image `pytorch_23.09-py3.sif`. Then you can use the image as usual.

## PyTorch on Multi-Instance GPU (MIG)

The `gpu_mig` partition is setup with [Multi-instance GPU (MIG)](https://www.nvidia.com/en-us/technologies/multi-instance-gpu/) feature of Nvidia A100s. If you would like to use PyTorch on `gpu_mig`, please [send us a ticket](https://docs.rc.fas.harvard.edu/kb/support/).


## References:

* [Official PyTorch website](https://pytorch.org)
* [PyTorch Tutorials](https://pytorch.org/tutorials/)
