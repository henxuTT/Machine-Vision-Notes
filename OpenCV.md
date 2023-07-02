# 图像基本操作

## 图像读取,显示,保存

```python
cv2.IMREAD_COLOR
cv2.IMREAD_GRAYSCALE
```

```python
import cv2     #BGR

#彩色图  窗口显示与销毁
img = cv2.imread('cat.jpg')
cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()

#灰度图
img = cv2.imread('cat.jpg', cv2.IMREAD_GRAYSCALE)

#保存
cv2.imwrite('my_cat.png', img)
```

```python
def cv_show(name, img):
	cv2.imshow(name, img)
	cv2.waitKey(0)
	cv2.destroyAllWindows()
```



---

## 视频读取

```python
cv2.VideoCapture('xxx.mp4')
cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

vc.isOpend()
open, frame = vc.read()
vc.release()
```

```python
vc = c2.VideoCapture('test.mp4')

#检查是否打开争取
if vc.isOpened():
    open, frame = vc.read()
else
	open = False
    
while open:
    ret, frame = vc.read()
    if frame is None:
        break
    if ret == True:
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        cv2.imshow('result', gray)
        if cv2.waitKey(10) & 0xFF == 27
        	break
vc.release()
cv2.destroyAllWindows()
```



---

## ROI区域

### 图像切割

```sh
cat = img[:200, 0:200]
```

 ### 颜色通道提取

```sh
cv2.split(img)
cv2.merge((b, g, r))
```

```shell
b,g,r = cv2.split(img)
img = cv2.merge((b, g, r))

#只保留R
cur_img = img.copy()
cur_img[:, :, 0] = 0
cur_img[:, :, 1] = 0
cv_show('G', cur_img)
```



---

## 边界填充

+ replicate                            复制边缘
+ reflect                                反射堆成
+ reflect_101                        
+ wrap                                   重复填充
+ constant

```sh
cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, borderType=cv2.BORDER_REPLICATE)
```



---

## 数值计算

```python
img + img2  		   #超过255部分取余
cv2.add(img, img2)     #超过255部分取255
```

```sh
img = cv2.resize(img, (500,414))
img = cv2.resize(img, (0, 0), fx=1, fy=3)      #比例缩放
```



# 图像进阶操作

---

## 图像阈值

```python
ret, dst = cv2.threshold(src, thresh, maxval, type)
```

+ ret:  所选阈值
+ src:  通常为单通道，灰度图
+ maxval: 达到阈值条件赋予的值
+ type: 二值化操作类型，包含以下五种
  + THRESH_BINARY                             #超过阈值取maxval， 否则取0
  + THRESH_BINARY_INV                     #反转
  + THRESH_TRUNC                             #超过阈值取maxval， 否则不变
  + THRESH_TOZERO                           #大于阈值不变，否则为0
  + THRESH_TOZERO_INV                   #大于阈值设为0，否则不变

## 图像平滑处理

### 均值滤波

简单的平均卷积操作

```python
blur = cv2.blur(img, (3, 3))
```

### 方框滤波

基本同均值，可选择归一化

```python
box = cv2.boxFilter(img, -1, (3,3), normalize=True)
# -1 表示深度相同
# 不归一化容易越界（相加超255，会全取255）
```

### 高斯滤波

高斯模糊的卷积核数值满足高斯分布，越靠近中间比重越大

```python
gaussian = cv2.GaussianBlur(img, (5, 5), 1)
```

### 中值滤波

取中间值做结果

```python
median = cv2.medianBlur(img, 5)
```



---

# 形态学

## 腐蚀操作

```python
kernel = np.ones((5, 5), np.int8)
erosion = cv2.erode(img, kernel, iterations = 1)
```

## 膨胀操作

```python
dilate = cv2.dilate(img, kernel, iterations = 1)
```

## 开运算，闭运算

+ 开：先腐蚀，在膨胀
+ 闭：先膨胀，再腐蚀

```python
dst = cv2.morphologyEx(src, op, kernel, dst, anchor, iterations, borderType, borderValue)
```

+ op: 形态学操作
  + `cv2.MORPH_OPEN`：开运算，先腐蚀后膨胀。
  + `cv2.MORPH_CLOSE`：闭运算，先膨胀后腐蚀。
  + `cv2.MORPH_GRADIENT`：形态学梯度，膨胀图像减去腐蚀图像。
  + `cv2.MORPH_TOPHAT`：顶帽运算，输入图像减去开运算结果。
  + `cv2.MORPH_BLACKHAT`：黑帽运算，闭运算结果减去输入图像。

```python
opening = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)
closing = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)
```

---

## 梯度计算

膨胀-腐蚀

```python
gradient = cv2.morphologyEx(img, c2.MORPH_GRADIENT, kernel)
```

## 礼帽与黑帽

礼帽 = 原始输入-开运算   （刺条）

黑帽 = 闭运算-原始输入      （填洞）

```python
tophat = cv2.morphologyEx(img, cv2.MORPH_TOPHAT, kernel)
blackhat = cv2.morphologyEx(img, cv2.MORPH_BLACKHAT, kernel)
```





---

# 梯度运算

## Sobel算子

![image-20230630235952646](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230630235952646.png)

```python
dst = cv2.Sobel(src, ddepth, dx, dy, ksize, scale, delta, borderType)

#示例
sobelx = cv2.sobel(img, cv2.CV_64F, 1, 0, ksize=3)
sobelx = cv2.convertScaleAbs(sobex)

#建议分开算x， y， 再求和
sobely = cv2.sobel(img, cv2.CV_64F, 0, 1, ksize=3)
sobelxy = cv2.addWeighted(sobelx, 0.5, sobely, 0.5, )
```

+ ddepth: 输出图像深度，-1表示相同

  + cv2.CV_64F: 表示图像深度（depth）的一个常量。它表示一个 64 位浮点数（float64）

  + 在使用 `cv2.Sobel()` 函数计算图像的梯度时，参数 `ddepth` 用于指定输出图像的深度。通过设置 `ddepth` 参数为 `cv2.CV_64F`，可以确保输出的梯度图像使用 64 位浮点数表示，从而保留梯度的精度和范围。

  + 需要注意的是，在计算梯度时，通常将输入图像转换为浮点数类型，以避免梯度计算过程中的截断错误。因此，选择 `cv2.CV_64F` 作为梯度图像的深度是一种常见的做法。

  + 负数会被截断成0，所以方便后续取绝对值

+ dx, dy: 导数阶数，0表示不计算此方向上的导数

##  Scharr算子

![image-20230701152454191](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230701152454191.png)

```python
scharrx = cv2.Scharr(img, cv2.CV_64F, 1, 0)
scharrx = cv2.convertScaleAbs(scharrx)
```



## Laplacian算子

对噪声敏感抗性差

![image-20230701152610674](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230701152610674.png)

```s
laplacian = cv2.Laplacian(img, cv2.CV_64F)
laplacian = cv2.convertScaleAbs(laplacian)
```



# Canny边缘检测

+ 使用高斯滤波器，平滑图像，消除噪声
+ 计算每个像素点梯度与方向
+ 应用非极大值抑制，消除边缘检测带来的杂散效应**(Non-Maximum Suppression)**
+ 应用双阈值检测来确定真实的和潜在的边缘**(Double-Threshold)**
+ 抑制孤立的弱边缘，最终完成边缘检测

## 非极大值抑制

### 线性插值法

Linear Interpolation

### 简化

一个像素周围八个像素，梯度离散为8个方向，取最近角度，比较前后两个像素点

## 双阈值检测

maxVal, minVal

+ 梯度值大于maxVal，则处理为边界
+ 梯度值之间： 连有边界则保留，否则舍弃
+ 梯度值小于minVal，舍弃

![image-20230701155252518](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230701155252518.png)

## Canny函数

```python
cv2.Canny(image, threshold1, threshold2, edges, apertureSize, L2gradient)

v1 = cv2.Canny(img, 80, 150)
```

+ threshold1:   低阈值
+ threshold2：高阈值
+ edges (可选): 输出边缘图像，单通道灰度图像，其中非零像素表示边缘。
+ apertureSize (可选): Sobel算子的孔径大小，默认为3。
+ L2gradient (可选): 指定梯度计算使用的公式，False表示使用L1范数，True表示使用L2范数，默认为False





---

# 图像金字塔

## 高斯金字塔

向下采样发（缩小）

+ 高斯内核卷积
+ 去除所有偶数行和列

向上采样法（放大）

+ 每个方向扩大两倍，0填充
+ 先前内核x4，再卷积，获得近似值

```sh
dst = cv2.pyrUp(src[, dstsize[, borderType]])
up = cv2.pyrUp(img)
down = cv2.pyrDown(img)
```

- src: 输入图像，可以是单通道或多通道的灰度图像或彩色图像。
- dstsize (可选): 输出图像的大小，可以指定为元组 (width, height)。如果不指定，输出图像的大小将是输入图像的尺寸的两倍。
- borderType (可选): 边界模式，用于处理边缘像素的情况。默认为cv2.BORDER_DEFAULT

## 拉普拉斯金字塔

![image-20230701165448832](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230701165448832.png)

# 图像轮廓

**Image Contour**

``` python
cv2.findCountours(img, mode, method)

contours, hierarchy = cv2.findContours(image, mode, method[, contours[, hierarchy[, offset]]])

gray = cv2.cvtColor(img, cv2.BGR2GRAY)
ret, thresh = cv2.threshold(gray, 127, 255, cv2.THRESHBINARY)
binary, contours, hierarchy = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
```

- image: 输入图像，通常是一个二值化图像（黑白图像）

- mode: 轮廓检索模式，指定轮廓的层次结构关系。常见的模式有

  - cv2.RETR_EXTERNAL（只检测外部轮廓）
  - cv2.RETR_LIST（检测所有轮廓，不建立层次关系）
  - cv2.RETR_CCOMP：检测所有轮廓，并将其组织为两层的层次结构。顶层是各个外部轮廓，次层是各个内部轮廓。
  - cv2.RETR_TREE：检测所有轮廓，并将其组织为完整的层次结构树(**常用**)

- method: 轮廓近似方法，指定轮廓的近似方式。常见的方法有

  - cv2.CHAIN_APPROX_SIMPLE（仅保留端点，压缩水平、垂直和对角线）(**常用**)
  - cv2.CHAIN_APPROX_NONE（保留所有的轮廓点）

  

- contours (可选): 输出参数，存储检测到的轮廓。每个轮廓表示为一个点的列表

- hierarchy (可选): 输出参数，存储轮廓的层次结构信息

  ![image-20230701173852289](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230701173852289.png)

## 绘制轮廓

```python
image = cv2.drawContours(image, contours, contourIdx, color[, thickness[, lineType[, hierarchy[, maxLevel[, offset]]]]])

res = cv2.drawContours(img, countours, -1, (0, 0, 255), 2)
```

- `image`: 输入图像，在该图像上绘制轮廓。
- `contours`: 要绘制的轮廓列表，通常是通过 `cv2.findContours` 函数获得的结果。
- `contourIdx`: 要绘制的轮廓在轮廓列表中的索引。如果为负数，则绘制所有的轮廓。
- `color`: 绘制轮廓的颜色，可以是一个BGR元组（例如 `(255, 0, 0)` 表示蓝色）。
- `thickness` (可选): 轮廓线的粗细。如果为负数或未指定，则绘制填充的轮廓。
- `lineType` (可选): 轮廓线的类型，可以是 `cv2.LINE_8`、`cv2.LINE_4` 或 `cv2.LINE_AA`。
- `hierarchy` (可选): 轮廓的层次结构信息，通常是通过 `cv2.findContours` 函数获得的结果。
- `maxLevel` (可选): 绘制的最大轮廓层级。如果为0，则绘制所有轮廓。如果为1，则只绘制顶层轮廓。

避免改变原始图片

```python
draw_img = img.copy()
res = cv2.drawCountours(draw_img, countours,0, (0, 0, 225), 2)
```

## 轮廓特征

**Counter Features**

取出各层轮廓

```python
cnt = contours[0]
```

面积

```python
cv2.contourArea(cnt)
```

周长，true表示闭合

```s
cv2.arcLength(cnt, True)
```

## 轮廓近似

**Contour Approximation**

OpenCV 中的轮廓近似方法是基于 Douglas-Peucker 算法，也称为 Ramer-Douglas-Peucker 算法。该算法的基本思想是在保持轮廓形状大致相似的前提下，通过舍弃一些轮廓点来实现近似。它的过程如下：

1. 给定一个输入轮廓，选择轮廓上的起始点和结束点，构成一个线段。
2. 计算其他所有点到这条线段的距离，找到距离最大的点作为当前轮廓上的一个关键点。
3. 如果最大距离小于指定的阈值，则认为整条轮廓可以用一条直线近似表示，结束近似过程。
4. 如果最大距离大于等于阈值，则将最大距离点作为新的关键点，将原轮廓分成两部分。
5. 对新生成的两部分轮廓分别重复步骤 1 到 4，直到满足结束条件。

```python
# 对每个轮廓进行近似
epsilon = 0.02 * cv2.arcLength(contours[0], True)			#系数按周长百分比
approx = cv2.approxPolyDP(contours[0], epsilon, True)

# 在图像上绘制原始轮廓和近似轮廓
cv2.drawContours(image, [contours[0]], -1, (0, 0, 255), 2)
cv2.drawContours(image, [approx], -1, (0, 255, 0), 2)
```

## 边界矩形

**Bounding Rectangle**

```s
x, y, w, h = cv2.boundingRect(contour)
cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)
```

- `x`: 外接矩形左上角点的 x 坐标
- `y`: 外接矩形左上角点的 y 坐标

## 模板匹配

**Template Matching**

模板匹配的基本原理是将模板图像在目标图像上滑动，并计算模板与目标图像局部区域的相似度
