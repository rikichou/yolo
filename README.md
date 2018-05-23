
# 1. 基于YOLO的人脸检测与识别

在很多大型企业的门口，往往都有人脸识别的闸机，只要人脸通过了视频验证就可以通过闸机，完成打卡的功能！

受此启发，决定使用深度学习技术来实现一个人脸识别的闸机，这个人脸闸机的大致工作流程如下：

    1. 摄像头采集视频数据
    2. 将摄像头采集到的图像帧输入到人脸检测模块，人脸检测模块输出图片中所有的人脸（可以设置限制只输出一张人脸）
    3. 将人脸检测模块输出的人脸图像输入到人脸识别模块完成身份识别
    
本项目使用了darknet，keras，tensorflow等深度学习框架，编程语言为Python，该项目更像是一个教程，为了便于描述，所以代码以jupyter notebook的形式呈现，详细介绍了必要的步骤！

# 2. 流程

描述一下应该如何来看这个项目！

## 2.1 了解darknet--YOLO_face_detection_and_recognition_1--first_sight_of_darknet.ipynb

有必要介绍介绍darknet，因为我们的项目中会利用darknet来训练YOLO的人脸检测模型。在本notebook中描述了darknet的基本使用方法，如果已经熟悉该深度学习框架的朋友可以略过。

## 2.2 使用darknet训练YOLO人脸检测模型--YOLO_face_detection_and_recognition_2--train_yolo_on_CelebA_datasheet.ipynb

因为我们要实现人脸识别，所以肯定需要把人脸从图片中检测出来。本notebook中，首先介绍了如何使用darknet在VOC数据集上训练YOLO的目标检测模型，主要是为了让大家熟悉darknet训练的流程！接着再言归正传，介绍了如何使用darknet在人脸数据集CelebA上训练YOLO模型，这里我的训练环境是亚马逊的p2.xlarge云服务器，配置为Tesla K80显卡，12G显存，训练了大概7个小时，5000个batch，大家可以参考一下是否需要自己训练，我将训练好的权重数据存放在另一个github，notebook中有介绍。

另外因为有大量的数据集需要上传到云服务器，所以还介绍了如何上传大量的数据到云服务器！

## 2.3 facenet搭建人脸识别模型--YOLO_face_detection_and_recognition_3--YAD2K_and_facenet.ipynb

在这个notebook中，主要介绍了如果如何将YOLO模型从darknet移植为keras+tensorflow版本。接着介绍了人脸识别中涉及到的概念，Siamese网络以及triplet loss等。然后介绍了人脸验证和人脸识别的关系。最后，利用人脸检测+人脸识别搭建了最终的人脸识别闸机系统！

# 3. 效果视频演示

特地拍摄了一段视频来演示人脸识别的效果（学习为主，请忽略本人长相）

优酷地址：http://v.youku.com/v_show/id_XMzYyMzA2NTI5Ng==.html?spm=a2hzp.8244740.0.0  
YouTube地址：https://www.youtube.com/watch?v=ugltndM89G4

如有疑问欢迎邮件沟通：
    
    zhourui_cq@163.com  
    657708642@qq.com

# 参考资料

Darknet: [Open Source Neural Networks in C](https://pjreddie.com/darknet/)  
VOC数据集：[Pascal VOC](http://host.robots.ox.ac.uk/pascal/VOC/)  
CelebA数据集：[CelebA](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)  
YOLO：[YOLO](https://pjreddie.com/darknet/yolov2/)  
keras：[keras](https://keras.io/)  
deepface：[DeepFace: Closing the Gap to Human-Level Performance in Face Verification](https://www.cs.toronto.edu/~ranzato/publications/taigman_cvpr14.pdf)  
facenet：[FaceNet: A Unified Embedding for Face Recognition and Clustering](https://arxiv.org/abs/1503.03832)  
YAD2K：[YAD2K](https://github.com/allanzelener/YAD2K)

