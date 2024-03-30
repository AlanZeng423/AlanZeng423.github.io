---
title: 'CV_filter'
date: 2024-3-30
permalink: /posts/CV_filter/
tags:
  - cool posts
  - CV
  - category2
---


#3 Filter

### Image as Function

### Image Noise

Common Types of Noise

- Impulse Noise (脉冲噪声)
  - Salt Noise: Random bright (with 255 pixel value) all over the image.
  - Pepper Noise: Random dark (with 0 pixel value) all over the image.
  - Salt and Pepper Noise: Both random bright (with 255 pixel value) and random dark (with 0 pixel value) all over the image.
- Gaussian noise (高斯噪声)
  - Variations in intensity drawn from a Gaussian normal distribution
  - Gaussian normal distribution: $ p(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}} $
  - $ f(x,y) = f(x,y) + n(x,y) ~其中 ~n(x,y) \sim N(\mu,\sigma^2) $
  - change mean or sigma



### Noise Reduction

Moving Average

- 1D

- 2D

- 

#### Correlation filtering

> If the averaging window size is $2 k+1 \times 2 k+1$

  - Uniform weights

$$
  G[i, j]=\frac{1}{(2 k+1)^2} \sum_{u=-k}^k \sum_{v=-k}^k F[i+u, j+v]
$$

  - Non-Uniform weights:

$$
G[i, j]=\sum_{u=-k}^k \sum_{v=-k}^k H[u, v] F[i+u, j+v]
$$

#### Averaging filter 均值滤波

- Mask with positive entries that sum to 1

- Replaces each pixel with an average of its neighborhood

- All weights are equal

  <img src="https://raw.githubusercontent.com/AlanZeng423/Pics/main/MarkDown_Pics/image-20240322153546056.png" alt="image-20240322153546056" style="zoom:33%;" align="left"/>



- Drawback: smoothing reduces fine image detail

- code

  ```c++
  void cv::blur(Mat InputArray src,  //原始图像
                Mat OutputArray dst,  //目标图像
                Size 	ksize,  //滤波模板尺寸
                Point anchor = Point(-1,-1),  
                //锚点（此刻被滤波处理的点）默认为内核中心
                int borderType = BORDER_DEFAULT  
                //扩展边界的类型，默认值BORDER_DEFAULT
  			  )	
  ```

  

#### Gaussian filter 高斯滤波

- Weighted averaging

- The coefficients are a 2D Gaussian.

- Gives more weight at the central pixels and less weights tothe neighbors.

- The farther away the neighbors, the smaller the weight.

- Parameters matters:

  - Size of kernel or mask

  - Variance of Gaussian: determines extent of smoothing 高斯方差:决定平滑程度

    <img src="https://raw.githubusercontent.com/AlanZeng423/Pics/main/MarkDown_Pics/image-20240322153949169.png" alt="image-20240322153949169" style="zoom:50%;" />

- Boundary issues

- code

  ```c++
  void GaussianBlur(Mat InputArray src,  //原始图像
  				  Mat OutputArray dst,  //目标图像
                    Size ksize,  //高斯核尺寸
                    double sigmaX,  //高斯核在X方向上的标准差
                    double sigmaY=0,  //高斯核在Y方向上的标准差
                    int borderType=BORDER_DEFAULT  
                    //边界模式,默认值BORDER_DEFAULT       
                    )
  
  ```



#### Sharpening filter 锐化滤波

- Accentuates differences with local average



#### Median filter 中值滤波

- No new pixel values introduced

- Removes spikes: good for impulse, salt & pepper noise (移除尖刺:有利于脉冲，盐和胡椒噪音)

  <img src="https://raw.githubusercontent.com/AlanZeng423/Pics/main/MarkDown_Pics/image-20240322155026102.png" alt="image-20240322155026102" style="zoom: 33%;" />

- code

  ```c++
  void medianBlur(Mat InputArray src,   //原始图像
  			    Mat OutputArray dst,  //目标图像
  			    int ksize   //滤波器尺寸
  			    )
  ```



#### Bilateral Filter 双边过滤器

- Non-linear, edge-preserving

- Depends on Euclidean distance and Radiometric differences(取决于欧几里得距离和辐射差)

- The bilateral filter is defined as:
  $$
  \begin{aligned}
  & I^{\text {filtered }}(x)=\frac{1}{W_p} \sum_{x_i \in \Omega} I\left(x_i\right) f_r\left(\left\|I\left(x_i\right)-I(x)\right\|\right) g_s\left(\left\|x_i-x\right\|\right) \\
  & W_p=\sum_{x_i \in \Omega} f_r\left(\left\|I\left(x_i\right)-I(x)\right\|\right) g_s\left(\left\|x_i-x\right\|\right)
  \end{aligned}
  $$

- code

  ```c++
  void bilateralFilter(Mat InputArray src,  //原始图像
  				     Mat OutputArray dst,  //目标图像
  				     int d,  //过滤过程中每个像素邻域的直径
  				     double sigmaColor,  //颜色空间滤波器sigma的值
  				     double sigmaSpace,  //坐标空间中滤波器sigma的值
  				     int borderType=BORDER_DEFAULT  //边界类型，默认值BORDER_DEFAULT
  				     )
  ```

  

## 5 Feature Detection and matching

### Image Matching

### Harris Corner Detection

### Scale Invariant Detection

Solution: Design a function which is Scale Invariant

- 单峰函数

Laplacian of Gaussian: $L = \sigma^2 (G_{xx}(x,y,\sigma)+G_{yy}(x,y,\sigma))$

$$
LoG(x,y,\sigma) = -\frac{1}{\pi \sigma^4} (1-\frac{x^2+y^2}{2\sigma^2})e^{-\frac{x^2+y^2}{2\sigma^2}} \\

DoG(x,y,\sigma) = G(x,y,k\sigma)-G(x,y,\sigma)
$$

Laplacian of Gaussian for Scale Invariant Detection:

- 1. Compute the Laplacian of Gaussian for a range of $\sigma$
- 2. Find the local maxima in the scale space
- 3. Output the location and scale of each detected feature
- 4. The scale of the feature is the $\sigma$ at which the feature is detected
- 5. The location of the feature is the location of the maxima in the scale space


#### SIFT

Scale Invariant Feature Transform

1. Scale Space Extrema Detection(尺度空间极值检测)
   - Find robust extremum(Maxima and Minima) both in the scale and space
   - $D = [log_2(min(M,N))] - 3$
   - 高斯金字塔:  
2. Keypoint Localization
   - Compute the Taylor expansion of the scale space
   - Eliminate low contrast points
     - Peak has 
3. Orientation Assignment