# CAX: Cellular Automata Accelerated

CAX is a high-performance cellular automata library built on top of JAX/Flax that is **designed for flexiblity**.

## Overview 🔎

CAX is a cutting-edge library designed to implement and accelerate various types of cellular automata using the JAX framework. Whether you're a researcher, a hobbyist, or just curious about the fascinating world of emergent and self-organizing systems, CAX has got you covered! 🧬

Despite their conceptual simplicity, cellular automata often demand significant computational resources. The parallel update of numerous cells, coupled with the need for backpropagation through time in neural cellular automata training, can render these models computationally intensive. CAX leverages hardware accelerators and massive parallelization to run cellular automata experiments in minutes. 🚀

The library works with discrete or continuous cellular automata of any spatial dimension, offering exceptional flexibility. From simulating one-dimensional [elementary cellular automata](https://github.com/879f4cf7/cax/blob/main/examples/elementary_ca.ipynb) to training three-dimensional [self-autoencoding neural cellular automata](https://github.com/879f4cf7/cax/blob/main/examples/self_autoencoding_mnist.ipynb), and even creating beautiful [Lenia simulations](https://github.com/879f4cf7/cax/blob/main/examples/lenia.ipynb), CAX provides a versatile platform for exploring the rich world of self-organizing systems. ✨

## Implemented Cellular Automata 🦎

| Cellular Automata | Reference | Example |
| --- | --- | --- |
| Elementary Cellular Automata | [Wolfram, Stephen (2002)](https://www.wolframscience.com/nks/) | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/elementary_ca.ipynb) |
| Conway's Game of Life | [Gardner, Martin (1970)](https://web.stanford.edu/class/sts145/Library/life.pdf) | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/life.ipynb) |
| Lenia | [Chan, Bert Wang-Chak (2020)](https://arxiv.org/pdf/2005.03742) | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/lenia.ipynb) |
| Growing Neural Cellular Automata | [Mordvintsev, et al. (2020)](https://distill.pub/2020/growing-ca/) |[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/growing_nca.ipynb) |
| Growing Conditional Neural Cellular Automata | [Sudhakaran et al. (2020)](http://arxiv.org/abs/2205.06806) | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/growing_conditional_nca.ipynb) |
| Growing Unsupervised Neural Cellular Automata | [Palm et al. (2021)](https://arxiv.org/abs/2201.12360) | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/growing_unsupervised_nca.ipynb) |
| Self-classifying MNIST Digits | [Randazzo, et al. (2020)](https://distill.pub/2020/selforg/mnist/) |[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/self_classifying_mnist.ipynb) |
| Diffusing Neural Cellular Automata | New | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/diffusing_nca.ipynb) |
| Self-autoencoding MNIST Digits | New | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/self_autoencoding_mnist.ipynb) |
| 1D-ARC Neural Cellular Automata | New | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/879f4cf7/cax/blob/main/examples/1d_arc_nca.ipynb) |

## Installation 🛠️

You will need Python 3.10 or later, and a working JAX installation.

Then, install CAX from PyPi:
```
pip install cax
```

To upgrade to the latest version of CAX, you can use:
```
pip install --upgrade git+https://github.com/879f4cf7/cax.git
```

## Getting Started 🚦

```python
import jax
from cax.core.ca import CA
from cax.core.perceive.depthwise_conv_perceive import DepthwiseConvPerceive
from cax.core.update.nca_update import NCAUpdate
from flax import nnx

seed = 0

channel_size = 16
num_kernels = 3
hidden_size = 128
cell_dropout_rate = 0.5

key = jax.random.key(seed)
rngs = nnx.Rngs(seed)

perceive = DepthwiseConvPerceive(channel_size, rngs)
update = NCAUpdate(
	channel_size,
	num_kernels*channel_size,
	(hidden_size,),
	rngs,
	cell_dropout_rate=cell_dropout_rate
)
ca = CA(perceive, update)

state = jax.random.normal(key, (64, 64, channel_size))
state = ca(state, num_steps=128)
```
