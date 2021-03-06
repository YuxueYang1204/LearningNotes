# OpenCV知识点

## 像素读取

```C
Mat src(3, 3, CV_8UC1);			// 创建单通道src
int val = src.at<uchar>(row, col);	// 仅单通道才能用uchar
Mat src2(src.size(), CV_8UC3);
int val2 = src2.at<Vec3b>(row, col)[0]；// 读取三通道，最后表示通道数，分别表示B、G、R
```

### Vec3b与Vec3f区别

- Vec3b对应三通道的uchar类型数据
- Vec3f对应三通道的float类型数据
- Vec{2,3,4,6}{b,w,s,i,f,d}可以任意组合

|缩写|全称|
|-|-|
|b|unsigned char|
|w|unsigned short|
|s|short|
|i|int|
|f|float|
|d|double|

## 图像混合

`addWeight(input1, alpha1, input2, alpha2, gamma, output)`
