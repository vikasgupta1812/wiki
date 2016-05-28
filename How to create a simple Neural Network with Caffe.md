How to create a simple Neural Network with Caffe
====


Issue - https://github.com/BVLC/caffe/issues/115

Can anyone give me some examples for that? for example. I want to build a deep feed-forward neural network. It's structure is data input - 2000 - 1000 - 500 - 128 - 64 output, the inner hidden layers are sigmoid or relu.


Take example/imagenet as example, this CONV1's botton is data layer.


Please refer to the [Caffe presentation](), the models/imagenet.prototxt example, and defining the LeNet/MNIST model for pointers.



### A Caffe Layer

name, type, and the connection structure (input blobs and output blobs)

```json
name: "conv1"
type: CONVOLUTION
bottom: "data"
top: "conv1"
```

layer-specific parameters

```json
convolution_param {
    num_output: 20
    kernel_size: 5
    stride: 1
    weight_filler {
        type: "xavier"
    }
}
```


Link to the Google Protobuffer Documentation - https://developers.google.com/protocol-buffers/docs/overview

The caffe layer and network definitions are at src/caffe/proto/caffe.proto.

caffe/python/draw_net.py can be used to draw the network picture as shown in this slide and the next ones.



### A Caffe Network

- LogReg
- LeNet
- ImageNet, Krizhevsky

The networks being visualized here can all be accessed under caffe/examples/.

```json
name: "dummy-net"
layers { name: "data" …}
layers { name: "conv" …}
layers { name: "pool" …}
    … more layers …
layers { name: "loss" …}
```

### Training a Caffe Net

```json
Write a solver protobuffer:
train_net: "lenet_train.prototxt"
base_lr: 0.01
momentum: 0.9
weight_decay: 0.0005
max_iter: 10000
snapshot_prefix: "lenet_snapshot"
solver_mode: GPU
```

You can also specify the learning rate decreasing policy (like exponentially, or inversely proportional to time). 

Check out `src/caffe/proto/caffe.proto` for details - the code itself gives good inline explanations on what individual fields mean.

Currently, Caffe supports training with `stochastic gradient descent` with `minibatches` and `momentum` - probably the most widely used training method for deep networks. You can also write your own solver - check out `caffe/src/caffe/solver.cpp` for details.



### End to End Recipe...

- Convert the data to Caffe-format - leveldb, hdf5/.mat, list of images, LMDB, etc.
- Write a Network Definition
- Write a Solver Protobuffer text
- Train with the provided train_net tool `build/tools/train_net.bin` `solver.prototxt`


#### Examples are your friends
- `caffe/examples/mnist`,`cifar10`,`imagenet`
- `caffe/tools/*.bin`

To convert the data, `caffe/tools/extra/` may contain multiple scripts that are useful.

Make sure you also check out the mnist, cifar and imagenet tutorials on the caffe webpage - they provide step-by-step guides on how to run such examples.


### Peeking into Networks
The sample ipython notebook is at caffe/examples/filter_visualization.ipynb

While you are at it, also checkout the imagenet classification tutorial at caffe/examples/imagenet_classification.ipynb

Both could be easily viewed online using the nbviewer tool:

http://nbviewer.ipython.org/github/BVLC/caffe/blob/master/examples/filter_visualization.ipynb

http://nbviewer.ipython.org/github/BVLC/caffe/blob/master/examples/imagenet_classification.ipynb


https://github.com/BVLC/caffe/blob/master/examples/00-classification.ipynb
https://github.com/BVLC/caffe/blob/master/examples/01-learning-lenet.ipynb
https://github.com/BVLC/caffe/blob/master/examples/02-brewing-logreg.ipynb
https://github.com/BVLC/caffe/blob/master/examples/03-fine-tuning.ipynb
https://github.com/BVLC/caffe/blob/master/examples/detection.ipynb
https://github.com/BVLC/caffe/blob/master/examples/net_surgery.ipynb


### A Quick Sip of Brewed Models

http://demo.caffe.berkeleyvision.org/ (demo code to be open-sourced soon)

It is worth mentioning that there are several other demos online that use deep models for various tasks, some from startups:

http://horatio.cs.nyu.edu/
http://clarifai.com
http://rekognition.com/demo/concept


Of course, you can always find your personal photos with Google+ Photo's search functionality, which uses deep models (among other techniques) to understand image contents and provides a nice user experience.

### Transfer Learned Knowledge
Taking a pre-trained model and finetune it for related tasks

Multiple papers have discussed and demonstrated the power of such an approach - in Zeiler and Fergus's arXiv paper describing the ILSVRC 2013 winning result, our earlier paper on DeCAF, and Pierre Sermanet's OverFeat work, respectively:
[Zeiler-Fergus](http://arxiv.org/abs/1311.2901)
[DeCAF](http://arxiv.org/abs/1310.1531)
[OverFeat](http://arxiv.org/abs/1312.6229)


DeCAF is deprecated and is officially replaced by our new caffe implementation, but if you would like to check out the old good decaf, here it is:
https://github.com/UCB-ICSI-Vision-Group/decaf-release

A friendly note that the pretrained OverFeat is also available as a feature extractor at:
http://cilvr.nyu.edu/doku.php?id=software:overfeat:start

### Index of Downloadable files from project site
http://dl.caffe.berkeleyvision.org/
 



### Projects Using Caffe
[Expresso](https://github.com/val-iisc/expresso) is a Python-based GUI for designing, training and using CNNs. Expresso uses Caffe as its backend CNN framework. Some of its salient features :

- A convenient wizard-like interface to contextually guide the user during common scenarios such as data import, design and training of CNNs
- A smart-edit interface makes net creation easy and quick.
- Deep networks are color-coded and informatively presented
- Support for training external classifier (SVM) using deep features (i.e. features extracted by passing image data through pre-trained CNN and tapping output at layer(s) of CNN).
- Data Visualization

Visit the [project page](http://val.serc.iisc.ernet.in/expresso) for installation details and links to text/video tutorials.




References: 


- http://courses.cs.tau.ac.il/Caffe_workshop/Bootcamp/pdf_lectures/
- http://courses.cs.tau.ac.il/Caffe_workshop/Bootcamp/
- [Google Search](https://www.google.com/search?q=site%3Acourses.cs.tau.ac.il+inurl%3A%22Caffe_workshop%22&oq=site%3Acourses.cs.tau.ac.il+inurl%3A%22Caffe_workshop%22&aqs=chrome..69i57j69i58.429j0j4&sourceid=chrome&es_sm=119&ie=UTF-8)

- http://deeplearning.net/tutorial/lenet.html
- http://ufldl.stanford.edu/wiki/index.php/UFLDL_Tutorial


- http://www.joyofdata.de/blog/neural-networks-with-caffe-on-the-gpu/
- [A Brief Introduction to the DNN/CNN Toolbox - Noppa](https://noppa.oulu.fi/noppa/kurssi/521010j/luennot/521010J_toolbox_intro.pdf)
- http://stackoverflow.com/a/11477815
- http://scicomp.stackexchange.com/q/5110
- Caffe for feed forward networks http://stackoverflow.com/questions/30942693/caffe-for-feed-forward-networks
-  An Explanation of Xavier Initialization - http://andyljones.tumblr.com/post/110998971763/an-explanation-of-xavier-initialization
-  https://www.metacademy.org/roadmaps/rgrosse/deep_learning
-  http://deepdish.io/
-  https://groups.google.com/forum/#!topic/caffe-users/RPMRBVvz0ZA