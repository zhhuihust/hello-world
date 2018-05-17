Halcon备忘录

##目标管理
- 是否联通的终极图片检测
- 标定和选型打光记录


##2017-05-15
大概的扫完了用户手册，开始了解决方案手册的学习。
大概1000多页左右
##2017-05-16
开始solution_guide_i


##2015-05-17 (5/17/2018 2:07:12 PM )
###6 Edge Extration 
1. 像素级别的边缘提取

		read_image (Image, 'fuse')
		sobel_amp (Image, EdgeAmplitude, 'thin_sum_abs', 3)
		threshold (EdgeAmplitude, Region, 20, 255)
		skeleton (Region, Skeleton)

2. 霍夫变换(Hough Transform)
	> 1、霍夫变换检测直线原理
	> 
	>       霍夫变换，英文名称Hough Transform，作用是用来检测图像中的直线或者圆等几何图形的。
	> 
	>       一条直线的表示方法有好多种，最常见的是  y=mx+b 的形式。 假设有一幅图像，经过滤波，边缘检测等操作，变成了下面这张图的形状，怎么把这张图片中的直线提取出来。基本的思考流程是：如果直线 y=mx+b 在图片中，那么图片中，必需有N多点在直线上（像素点代入表达式成立），只要有这条直线上的两个点，就能确定这条直线。该问题可以转换为：求解所有的(m,b)组合。
    
3. 

