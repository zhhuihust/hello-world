OpenCV备忘录

##目标管理
- 实现人脸识别程序
- 配合C++写一些小程序
- 配合框架做一些分类算法实现



##2017-05-15
- 第一本书：OpenCV-Python教程
- 第二本书：OpenCV入门-毛星云


##2017-05-16
开始OpenCV-Python书的学习
#### 安装opencv 3.6
直接在cmd命令行输入：conda install opencv

CSDN博客地址:[window下anconda安装本地opencv](https://blog.csdn.net/u014403318/article/details/71237211](https://blog.csdn.net/u014403318/article/details/71237211)

####加载图像

	cv2.imshow('image',img)
	cv2.waitKey(0)
	cv2.destroyAllWindows()

####读入摄像头并显示蓝色物体
		cap=cv2.VideoCapture(0)
		while(1):
		    ret,frame = cap.read()
		    hsv=cv2.cvtColor(frame,cv2.COLOR_BGR2HSV)
		    lower_blue=np.array([110,50,50])
		    upper_blue=np.array([130,255,255])
		    mask=cv2.inRange(hsv,lower_blue,upper_blue)
		    res=cv2.bitwise_and(frame,frame,mask=mask)
		    cv2.imshow('frame',frame)
		    cv2.imshow('mask',mask)
		    cv2.imshow('res',res)
		    k=cv2.waitKey(5)&0xFF
		    if k==27:
		        break
		cv2.destroyAllWindows()

###14 几何变换
####目标


- 学习对图像进行各种几个变换，例如移动，旋转，仿射变换等。
- 将要学到的函数有：cv2.getPerspectiveTransform。
变换
- OpenCV 提供了两个变换函数，cv2.warpAffine 和cv2.warpPerspective，
- 使用这两个函数你可以实现所有类型的变换。cv2.warpAffine 接收的参数是2*3 的变换矩阵，而cv2.warpPerspective 接收的参数是3*3 的变换矩阵。
- 在缩放时我们推荐使用cv2.INTER_AREA，在扩展时我们推荐使用v2.INTER_CUBIC（慢) 和v2.INTER_LINEAR。默认情况下所有改变图像尺寸大小的操作使用的插值方法都是cv2.INTER_LINEAR。
- egg:放大图像

		import cv2
		import numpy as np
		img=cv2.imread('messi5.jpg')
		# 下面的None 本应该是输出图像的尺寸，但是因为后边我们设置了缩放因子
		# 因此这里为None
		res=cv2.resize(img,None,fx=2,fy=2,interpolation=cv2.INTER_CUBIC)
		#OR
		# 这里呢，我们直接设置输出图像的尺寸，所以不用设置缩放因子
		height,width=img.shape[:2]
		res=cv2.resize(img,(2*width,2*height),interpolation=cv2.INTER_CUBIC)
		while(1):
		cv2.imshow('res',res)
		cv2.imshow('img',img)
		if cv2.waitKey(1) & 0xFF == 27:
		break
		cv2.destroyAllWindows()
		
- egg：旋转图像

		img=cv2.imread('1.jpg',0)
		rows,cols=img.shape
		#旋转矩阵
		M=cv2.getRotationMatrix2D((cols/2,rows/2),120,0.7)
		#旋转
		dst=cv2.warpAffine(img,M,(cols,rows))
		while(1):
		    cv2.imshow('img',dst)
		    if cv2.waitKey(1)&0xFF==27:
		        break
		cv2.destroyAllWindows()

### 形态学
- 腐蚀

		img = cv2.imread('j.png',0)
		kernel = np.ones((5,5),np.uint8)
		erosion = cv2.erode(img,kernel,iterations = 1)

- 膨胀

		dilation = cv2.dilate(img,kernel,iterations = 1)

- 开运算

		先进行腐蚀再进行膨胀就叫做开运算。就像我们上面介绍的那样，它被用来去除噪声。这里我们用到的函数是		
		cv2.morphologyEx()
		
		opening = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)


- 闭运算

		先膨胀再腐蚀。它经常被用来填充前景物体中的小洞，或者前景物体上的
		小黑点。
		closing = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)

- 形态学梯度

		其实就是一幅图像膨胀与腐蚀的差别。
		结果看上去就像前景物体的轮廓。
		gradient = cv2.morphologyEx(img, cv2.MORPH_GRADIENT, kernel)
- 礼帽

		原始图像与进行开运算之后得到的图像的差。下面的例子是用一个9x9 的
		核进行礼帽操作的结果。
		tophat = cv2.morphologyEx(img, cv2.MORPH_TOPHAT, kernel)
- 黑帽

		进行闭运算之后得到的图像与原始图像的差。
		blackhat = cv2.morphologyEx(img, cv2.MORPH_BLACKHAT, kernel)




###Canny边缘检测


		import cv2
		import numpy as np
		from matplotlib import pyplot as plt
		img = cv2.imread('messi5.jpg',0)
		edges = cv2.Canny(img,100,200)
		plt.subplot(121),plt.imshow(img,cmap = 'gray')
		plt.title('Original Image'), plt.xticks([]), plt.yticks([])
		plt.subplot(122),plt.imshow(edges,cmap = 'gray')
		plt.title('Edge Image'), plt.xticks([]), plt.yticks([])
		plt.show()



###图像金字塔
####目标和原理
- 将要学习的函数有：cv2.pyrUp()，cv2.pyrDown()。
- 一般情况下，我们要处理是一副具有固定分辨率的图像。但是有些情况下，
我们需要对同一图像的不同分辨率的子图像进行处理。比如，我们要在一幅图
像中查找某个目标，比如脸，我们不知道目标在图像中的尺寸大小。这种情况
下，我们需要创建创建一组图像，这些图像是具有不同分辨率的原始图像。我
们把这组图像叫做图像金字塔（简单来说就是同一图像的不同分辨率的子图集
合）。如果我们把最大的图像放在底部，最小的放在顶部，看起来像一座金字
塔，故而得名图像金字塔。
有两类图像金字塔：高斯金字塔和拉普拉斯金字塔。
高斯金字塔的顶部是通过将底部图像中的连续的行和列去除得到的。顶
部图像中的每个像素值等于下一层图像中5 个像素的高斯加权平均值。这样
操作一次一个MxN 的图像就变成了一个M/2xN/2 的图像。所以这幅图像
的面积就变为原来图像面积的四分之一。这被称为Octave。连续进行这样
的操作我们就会得到一个分辨率不断下降的图像金字塔。我们可以使用函数
cv2.pyrDown() 和cv2.pyrUp() 构建图像金字塔。
函数cv2.pyrDown() 从一个高分辨率大尺寸的图像向上构建一个金子塔
（尺寸变小，分辨率降低）。


#### 实现图像融合的步骤
实现上述效果的步骤如下：

1. 读入两幅图像，苹果和橘子
2. 构建苹果和橘子的高斯金字塔（6 层）
3. 根据高斯金字塔计算拉普拉斯金字塔
4. 在拉普拉斯的每一层进行图像融合（苹果的左边与橘子的右边融合）
5. 根据融合后的图像金字塔重建原始图像。

####例程



