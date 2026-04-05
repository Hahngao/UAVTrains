# 实验03_飞机单目标定实验

## 1、实验背景

相机标定是确定相机的内在和外在参数的过程，这些参数用于将三维空间中的点映射到图像平面上。标定的主要目的是为了提高计算机视觉任务的精度，如 3D 重建、物体识别和定位等。未经标定的相机会因各种畸变（如镜头畸变）导致图像的几何失真，从而影响这些任务的准确性。例如：

1. 三维重建和测量：如果需要从2D图像中重建三维场景，必须知道相机的内外参数，才能将图像坐标转换为世界坐标。

2. 立体视觉系统：在双目视觉系统中，为了计算场景的深度信息，需要知道两个相机的相对位置和方向（外参），以及每个相机的内在参数（焦距、主点等）。

3. 机器人视觉：对于依赖视觉导航的机器人，相机标定有助于准确测量距离、识别和跟踪物体。

4. 增强现实（AR）和虚拟现实（VR）：在这些应用中，相机标定能保证虚拟对象在现实场景中的正确位置和比例。

5. 图像校正：相机标定能校正镜头的畸变，改善图像质量，使其更接近真实场景。

## 2、实验目的

### 1. 理解相机标定的基本原理

本实验可以让学生或研究人员掌握标定的基本流程，理解相机内外参的物理意义，以及如何通过实际图像数据提取并计算这些参数。

### 2. 掌握使用棋盘格进行相机标定

本实验可以使实验者理解如何通过提取棋盘格的角点来进行相机标定，并能够计算出相机的内参矩阵和畸变系数。

### 3. 提高图像处理与计算机视觉技能

本实验需要使用OpenCV 等计算机视觉库进行图像的处理与特征提取。实验者可以通过这个过程，熟悉图像预处理、角点检测、数据存储、相机标定算法等技能。

### 4. 获得相机内参

通过本实验，实验者能够计算出相机的内参矩阵（如焦距、主点位置）和畸变系数（如径向畸变、切向畸变）。

## 3、实验环境

<table><tr><td rowspan="2">序号</td><td rowspan="2">软件要求</td><td colspan="2">硬件要求</td></tr><tr><td>名称</td><td>数量(个)</td></tr><tr><td>1</td><td>Windows 10 及以上版本</td><td>笔记本①</td><td>1</td></tr><tr><td>2</td><td>Wifi 路由器</td><td></td><td>1</td></tr><tr><td>3</td><td>卓翼 310 飞机以及相关配件</td><td></td><td>1</td></tr><tr><td>4</td><td>场地设施配置</td><td></td><td>1</td></tr><tr><td>5</td><td>飞思视觉仿真平台</td><td></td><td>1</td></tr></table>


① ：推荐配置请见：https://rflysim.com/doc/zh/1/InstallLearn.html


## 4、实验步骤

### 步骤一：打印标定板

将标定板图像 calib.io_checker_200x150_6x6_20.pdf 打开，接着按照如图 1 所示格式打印即可。打印出来后固定在平整的板子上。

![image](pics/step1_pics/image1.png)



<center>图 1 标定板打印格式</center>




### 步骤二：连接 NoMachine

1. 在笔记本或者本地电脑端打开NoMachine进入连接页面如图2所示。

![image](pics/step1_pics/image2.png)



<center>图 2 NoMachine 连接页面图</center>




2. 点击左上角 add按钮进入编辑连接界面，在 Host处输入飞机 ip地址，点击右上角add后回到连接页面，如图3所示。

![image](pics/step1_pics/image3.png)



<center>图 3 NoMachine 编辑连接页面图</center>




3. 双击要连接的飞机 ip 后既可进入 NX 远程界面，如图 4 所示。如果显示连接问题具体请参考：问题 1。

![image](pics/step1_pics/image4.png)



<center>图 4 NoMachine 连接飞机</center>




4. 在账号密码界面输入账号nvidia和密码nvidia后点击OK后一直点击OK即可进入飞机远程。



![image](pics/step1_pics/image5.png)





<center>图 5 输入账号密码</center>




### 步骤三：启动传感器

1.进入/home/nvidia/Downloads/Calibrate_camera 文件夹，可以看到多个文件如图 6 所示。其中 sensor.sh 是开启相机的脚本， Calibrate.py 是进行单目相机标定的脚本， img 保存了单目相机标定时保存的图像。



![image](pics/step1_pics/image6.jpeg)

<center>图 6 sh 文件夹</center>



2.在 Calibrate 文件夹内，空白处，点击鼠标右键，打开一个终端，输入指令 ./sensor.sh如图 7所示。敲击回车，等待 30s后会打开D435i相机 如果输入 ./sensor.sh 出现问题可以查看问题4.

![image](pics/step1_pics/image7.jpeg)



<center>图 7 运行 sensor.sh 脚本文件</center>




### 步骤四：传感器数据检查

1. 运行传感器后需要查看传感器是否正确开启，对于前视相机正确开启后的消息打印如图8所示。

![image](pics/step1_pics/image8.jpeg)



<center>图 8 前视相机的启动</center>




### 步骤五：进行标定

1. 在 sh 文件夹下打开一个终端，输入指令 `python3 Calibrate.py` ，敲击回车。

![image](pics/step1_pics/image9.jpeg)



<center>图 9 Calibrate.py</center>




2. 如果 img 文件夹内没有图片，则需要手动采集，将标定板放置于相机前面，可以看到图像，使用鼠标点击一下图像窗口，此时按下s键，就可以保存图像，终端中会显示Saved img/img_X.png。采集图像时需要遵循原则：标定板按照三个欧拉角旋转，充分激励三个欧拉角。除此之外，还需要激励上下左右的平移，采集的图像如图 10所示。可以仿照给出

的图像激励三个欧拉角的旋转和上下左右平移。

![image](pics/step1_pics/image10.jpeg)









<center>图 10 采集的部分图像</center>




3. 当采集完一定数量的图像后就可以进行自动标定了，标定过程中会显示正在标定的图像如图12所示，标定完成后终端会打印出标定的结果。

![image](pics/step1_pics/image11.jpeg)



<center>图 11 单目标定</center>




![image](pics/step1_pics/image12.jpeg)



<center>图 12标定图像显示</center>




4. 如果 img 文件夹内有上次标定的图片，则不需要手动采集，此时会直接进行标定，标定过程中会显示正在标定的图像，标定完成后终端会打印出标定的结果。

![image](pics/step1_pics/image13.jpeg)



<center>图 13 单目标定结果</center>




### 步骤六：标定结果验证

1. 在标定完成后，我们需要衡量标定的结果是否符合使用标准。衡量的标准为重投影误差，重投影误差越小表明标定的结果越好，重投影误差衡量的标准如图14所示。

![image](pics/step1_pics/image14.jpeg)



<center>图 14 标定标准</center>




2. 在标定结果中会显示出标定的重投影误差如图 15所示，重投影误差为 0.28，标定质量较好。如果标定的重投影误差很大见问题3。



![image](pics/step1_pics/image15.jpeg)

<center>图 15 重投影误差</center>





## 6、常见问题

Q1：NoMachine进入不了飞机远程端怎么办？

A1：查看飞机是否和本地电脑连接的是同一个局域网并核对好飞机（NX）ip重新尝试。

Q2：如何查看前视相机是否正常开启？

A2：运行启动传感器脚本后，在任意位置打开一个终端，输入 rviz 打开 rviz，接着在选择图像话题 \camera\color\image_raw 中的 image。

![image](pics/step1_pics/image16.png)



<center>图 16 查看话题</center>




Q3：标定的重投影误差很大怎么办？

A3：首先检查一下使用的是我们提供的标定板图像，在选择图像时保证图像清晰并且标定板全部拍摄进去。

Q4：如果输入./sensor.sh 后出现 Permission Denied 报错问题怎么办？

![image](pics/step1_pics/image17.jpeg)



<center>图 17 没有权限图</center>




A4：这是因为文件可能是从 windows里面放进去的，没有权限。在终端中输入 sudo chmod777 * ，接着输入密码 nvidia 赋予所有文件权限即可。

![image](pics/step1_pics/image18.jpeg)



<center>图 18 赋予权限</center>

