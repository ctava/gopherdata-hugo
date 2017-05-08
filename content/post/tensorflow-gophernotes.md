+++
date = "2017-05-10T12:00:00"
draft = false
title = "Polyglot Machine Learning using Tensorflow: training in Python, execution using Golang"
math = true
summary = """Many platforms are a mix of programming languages. Its typically the case that data scientists are comfortable in R, Matlab and Python, where as, platform engineering is done in Ruby, Java, and Go. Irrespective of the tooling, or various skillsets involved, Tensorflow is enabling end-to-end systems."""

[header]
image = "headers/tensorflow-gophernotes.png"
caption = ""

+++

(Author: Chris Tava, @ctava1 on [Twitter](https://twitter.com/ctava1) and Gophers Slack)

 Tensorflow has organized machine learning into construction and execution phases. By separating machine learning into two phases - models trained during the construction phase can be re-used in an execution phase. This creates an end-to-end system thats works together despite the potential for tooling to have being written by different people, using different languages. 
 
 This post will walk you through a high-level description of Tensorflow. Then will step you through evaulation of a simple Tensoreflow/Go program inside a Jupyter Notebook. This program uses the Tensorflow API for loading a pre-trained model definition and will give you the ability to evaulate its results. 

**TensorFlow**:
TensorFlow is a programming system in which you represent computations as graphs. Nodes in the graph are called ops (short for operations). An op takes zero or more Tensors(a typed multi-dimensional array), performs some computation, and produces zero or more Tensors. To compute anything, a graph must be launched in a Session. A Session places the graph ops onto Devices, such as CPUs or GPUs, and provides methods to execute them.

**Why is it cool?**:
There are 3 things that are really cool about TensorFlow:

1. `models are explicit` - model definitions are explict and can be checkpointed and re-hydrated for later use.
2. `visualization` - tensorboard depicts scalars, graphs, histograms etc - which enables a visual understanding of machine learning programs 
3. `open source` - TensorFlow looks to have been designed for extensibilty because its contains protobufs defintion files and is encouraging language additions on top of the core C/C++ library. Also, you can read all of the source code in the [core framework](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/core/framework) etc. 

**Installation**:

To install and setup for this tutorial install [Docker](https://docs.docker.com/engine/installation/). 
Then pull down and run a docker image:

```
docker pull ctava/tensorflow-gophernotes:latest
docker run -p 8888:8888 -d tensorflow-gophernotes jupyter notebook --NotebookApp.token=token --no-browser --ip=0.0.0.0
```

and viola. There is a Jupyter notebook running on port 8888. You can access it [here](http://localhost:8888/tree/src/github.com/ctava/tensorflow-gophernotes).
Be sure to enter the token=token. Shout out to [Gophernotes](https://github.com/gopherdata/gophernotes) for enabling golang inside of Jupyter notebooks!

**Inside the notebook**:

Inside the notebook is a fun game: `Given a prefix sentence, ask the computer to generate the next words.`

The go program is loading a pre-trained [Tensorflow model](http://download.tensorflow.org/models/LM_LSTM_CNN/graph-2016-09-10.pbtxt). The model is a hybrid between character CNN, a large and deep LSTM, and a specific Softmax architecture which allowed the authors (Oriol Vinyals (vinyals@google.com, github: OriolVinyals), Xin Pan (xpan@google.com, github: panyx0718)) to train the model on the [One Billion Word Benchmark](https://arxiv.org/abs/1312.3005). Thus far, when compared to previous attempts, they have achived the lowest perplexity.

<screen cast>

**Visualization of Softmax using Tensorboard**:

To visualize softmax execution, obtain the running docker container id and open up bash:

```
docker ps
docker exec -it <container_id> bash

The issue the following command:
`tensorboard --logdir ./log`
```

You can access Tensorboard [here](http://localhost:6006).

<insert pic of tensorboard here>

**Playing around in TensorFlow**:
Also, feel free to play around with Tensorflow in golang. Here is an [image recognition](https://github.com/ctava/tensorflow-go-imagerecognition) example that included in the Docker image.


**Conclusion/Resources**:

I was able to quickly learn Tensorflow (in a couple of weekends) with the goal of re-using, executing and visualizing a model. To enable end-to-end development, hopefully the Tensorflow Go API will catch-up to the Python implementation. If your passionate about Golang and Machine Learning, there is now more of a reason to get involved and improve the tooling for Gophers.. 

- Visit [this repo](https://github.com/ctava/tensorflow-gophernotes) to get the sourcecode and docker image, so you can write your onw Tensorflow programs in Go!
- Join the [Gophers Slack - #data-science channel](https://invite.slack.golangbridge.org/) to get help implementing your ML pipelines, and participate in the discussion.
- Follow [GopherDataIO on Twitter](https://twitter.com/GopherDataIO), and
- [ctava1 on Twitter](https://twitter.com/ctava1).
- See the [Tensorflow go installation Instructions](https://www.tensorflow.org/install/install_go)
