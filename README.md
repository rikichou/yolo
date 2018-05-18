
# 1.初识darknet

第一次见到darknet是在学习YOLO之后，想要亲自实现一下[YOLO](https://pjreddie.com/darknet/yolov2/)，然后找YOLO的资料，就找到了[darknet](https://pjreddie.com/)的官网，当时心想，好家伙！为了为了一个YOLO专门搞了一个网站来介绍。当时就以为darknet就是一个YOLO模型的C语言实现（相信很多小伙伴跟我一样）。

后来发现不是这么简单的，其实darknet是一个深度学习框架，也就是和我们平常接触的[keras](https://keras.io/),tensorflow是同等作用。darknet和其它的深度学习框架不一样的是，这个深度学习框架是用C语言实现的，并且表现为一个可执行文件，不像其他深度学习框架那样对环境很依赖，这个特点使得它很容易被移植到嵌入式设备中，这也是它独有的一个特点。

# 2.darknet中使用YOLO

接下来我们就通过跑官网上的YOLO的例子来看看darknet究竟是个什么东西？（以下实验在Ubuntu下进行！！）

首先我们需要将darknet的源码克隆到本地，以便于之后的“安装”（啊！这么麻烦啊！我记得安装keras或者tensorflow只要一句话pip install就搞定了啊！没有办法，因为darknet是C语言实现的，所以我们需要将代码从git克隆到本地，进行源码级的安装）。

git clone https://github.com/pjreddie/darknet

然后进入darknet，执行make(其实就是使用相应的编译工具进行变异，生成可执行文件darknet)。注意这里编译方式有两种，具体采用哪一种取决于你的需求

第一种：直接编译，什么东西都不改，这样的话darknet将基于CPU运行，速度就不是很快了（其实是慢很多！）

cd darknet

make

第二种：修改makefile，使其在GPU（如果你有一张NVIDIA的显卡）下运行，其速度取决于不同的GPU，但是基本都比CPU快几十倍到几百倍左右。具体就是将Makefile中的前几行改为（在darknet目录下， sudo vim Makefile来进行修改）：

GPU=1

CUDNN=1

OPENCV=1（改这个是为了darknet和opencv一起编译，使得darknet可以输出图片、视频结果。但是注意在编译之前需要先使用sudo apt-get install libopencv-dev来安装opencv，否则会编译出错！）

另外需要注意的一点是，既然选择了在GPU下运行，那么必不可少的就是需要安装[CUDA](https://developer.nvidia.com/cuda-downloads),运行一下命令查看本机是否装有CUDA：

nvidia-smi

如果没有任何输出的话，那么就必须要安装CUDA了，安装过程官网很详细，就不重复了，只说一点过来人的忠告：一定要完全按照官网说的来，不然很容易白忙活几个小时！

修改完Makefile之后，直接make就好了！（如果你先使用了第一种方法编译，然后又想尝试第二种的话，那么在执行第二步的make操作之前，需要先运行make clean来清除之前编译的结果，总之小心使得万年船嘛）

## 2.1 运行官网的例子

上面的步骤运行完了之后，会在当前的目录（darknet目录）下生成一个可执行文件"darknet"，这就是我们的深度学习框架了！官网介绍了几个例子来利用darknet来搭建预训练的YOLO模型，下面我们就按照那个例子来解释一个darknet的运行机制。

如果现在我们想通过darknet来搭建YOLO模型，然后对一张图片进行目标检测，那么我们怎么做呢？如下：（请先看参数解释再运行命令）

./darknet detect cfg/yolov2.cfg yolov2.weights data/dog.jpg

下面我来解释一下每个参数的含义

    1：----./darknet，这个没什么好说的，就是运行框架
    
    2：----detect，表明我们是想做目标检测，而不是其他任务
    
    3：----cfg/yolov2.cfg，这个是我们要搭建的用于目标检测的模型的配置信息，里面包含了搭建一个模型所需要的所有信息。比如图片尺寸，优化算法以及参数，batch size，最终要的是里面包含了要搭建的模型的各层信息，比如卷积，池化等参数，所以你只要传入了这个文件，darknet就会按照这个配置进行模型的搭建。在cfg目录下还有很多其他模型的配置文件，比如vgg,resnet,alexnet等，感兴趣的可以自行查看
    
    4：----yolov2.weights，这个文件自然就是我们之前搭建的YOLOv2模型的权重了，由于这个文件比较大，所以darknet没有将其加入到工程中，需要自行下载，运行：  wget https://pjreddie.com/media/files/yolov2.weights将其下载到当前目录就好了
    
    5：----data/dog.jpg，这个就是我们要检测的图片，作为模型的输入，data目录下还有其他的图片，感兴趣的可以尝试一下。

运行完上面的命令之后就可以看到结果了，在终端中会显示模型的预测结果，以及预测时间。如果之前编译darknet的时候你修改了Makefile：OPENCV=1，那么此时就会显示一张带有预测bounding box的图片。不然不会显示，但输出的结果也会存放在当前目录下的predictions.png中

官网上还有关于将摄像头数据或者视频作为YOLO输入的命令，其实都很简单，请自行查看。

总结：所以就目前来看，darknet使用的方法很简单，只需要将模型的配置文件，以及模型的权重参数文件传输给darknet就好了。但是从yolov2.cfg文件中，我们可以看到有很多YOLO模型才会有的私有的配置，比如anchors，所以darknet并不是一个通用的深度学习框架，它只能搭建、训练一些他支持的模型（比如YOLO）以及一些通用的简单的模型（比如alexnet），并不能进行类似tensorflow那样的复杂操作。

好了，本notebook的目的就是为了让大家对darknet有一个初步的了解，如果你懂了，那我的目的就达到了。这只是我们搭建人脸识别系统的第一步，敬请期待后续更新！
