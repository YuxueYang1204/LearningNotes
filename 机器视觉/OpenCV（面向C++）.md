# OpenCV知识点

## 矩阵索引

### 单个元素索引

```C++
Mat src(3, 3, CV_8UC1);			// 创建单通道src
int val = src.at<uchar>(row, col);	// 仅单通道才能用uchar
Mat src2(src.size(), CV_8UC3);
int val2 = src2.at<Vec3b>(row, col)[0]；// 读取三通道，最后表示通道数，分别表示B、G、R
```

### 部分索引

```C++
Mat a = imread("test.jpg")
int center_x = a.rows / 2, center_y = a.cols / 2;
Mat left_top = a(Rect(0, 0, center_y, center_x));
```

注：`Rect(x, y, width, height)`中**x表示列坐标**，**y表示行坐标**，width表示列宽，height表示行高

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

注：unsigned char和unsigned int8(UINT8)是等价的

---

## 常用技巧

- 叠加两张图片可以用

```C++
addWeight(input1, alpha1, input2, alpha2, gamma, output)
```

- 查看数据类型

```C++
#include<typeinfo>
// 省略其他语句
Mat img = imread("test.jpg");
cout<<typeid(img.at<Vec3b>(0,0)[0]).name();
// 省略其他语句
```
