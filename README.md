借助Pytorch实现数据的在线增广
——————————————————————————————————————————
-------------------------------------------------------------------------------------------------------------------------------------------------------------
✔引言
---------------------------------------------------------------------------------------------------------------------------------------------------------
数据增广是深度学习中常用的技巧之一，通过对原有图片进行一定程度的随机变化（如旋转、缩放、翻转、裁剪）生成一组新的图片，从而扩大训练集的规模，让数据集尽可能的多样化。

1.视角变化：通过旋转、缩放、翻转等变换，让模型更好地识别目标，避免出现只在固定视角下能够正确识别的情况。

2.照明变化：通过改变亮度、对比度等参数，模拟不同的光照条件，使模型对光照条件的变化更加鲁棒。

3.遮挡变化：在图片中添加遮挡物，使模型能够更好地处理遮挡和复杂背景。

4.物体形变：通过变形、扭曲等变换，让模型更好地处理物体的形变，提高对不同形状物体的识别能力。

5.噪声变化：通过加入噪声、模糊等变换，让模型更加鲁棒，能够处理图像中的噪声或模糊情况。

✔四大数据增广需要解决的视觉问题
---------------------------------------------------------------------------------------------------------------------------------------
1.水平翻转or垂直翻转要解决平移不变性

2.随机旋转----旋转不变性

3.随机裁切----尺寸不变性

4.随机色度变换----光照复杂性

✔数据增广训练模型
-----------------------------------------------------------------------------------------------------------------------------------------
首先创建并配置好一个虚拟环境从GitHub克隆classification-basic-sample-master项目后，打开PyCharm找到train.py文件进行操作

![image](https://user-images.githubusercontent.com/128702185/229296707-ea0154ec-20dc-4229-b577-b5d0dd6c114e.png)

👉发现transforms.RandomResizedCrop了吗，这是随机裁切的意思👈 

-------------------------------------------------------------------------------------------------------------------------
👌常见的数据增强操作包括：

1.随机水平翻转（Horizontal Flip）

2.随机垂直翻转（Vertical Flip）

3.随机旋转（Random Rotate）

4.随机剪裁（Random Crop）

5.随机缩放（Random Scale）

6.随机加噪声（Random Noise）

7.颜色变换（Color Jittering）

8.数据标准化（Normalization）

9.随机改变图片对比度、亮度、饱和度等（Random Brightness/Contrast/Saturation）

------------------------------------------------------------------------------------------------------------------------------------------------
下面给出通过PyTorch中的transforms实现随机旋转和水平翻转的样例代码，你可以根据需要对参数进行修改

    train_transforms = transforms.Compose([
        transforms.RandomHorizontalFlip(p=0.5),
        transforms.RandomRotation(degrees=(-10, 10), fill=(0,)),
        transforms.ToTensor(),
        transforms.Normalize([0.5, 0.5, 0.5], [0.5, 0.5, 0.5])
    ])

其中，RandomHorizontalFlip表示随机水平翻转，RandomRotation表示随机旋转，degrees为旋转的角度范围，在这个例子中是-10~10度，fill表示填充颜色，这里使用0填充。p表示随机翻转的概率，这里是50%。

你需要在classification-basic-sample-master项目中创建一个data文件夹

修改好参数就可以进行测试训练了      ❗❗❗不光训练集需要修改，验证集也采用同样的增广❗❗❗
