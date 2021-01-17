  # Image Segmentation

    > An Image is worth thousand words.

  Image segmentation is the process of partitioning an image into multiple segments or regions. Image segmentation is done to change or simplify the representation of an image into segments that are more meaningful (to the human eye) and easy to analyze. Image Segmentation is used to locate objects and boundaries in the image. A segment is a group of pixels, so image segmentation is assigning a label to every pixel, such that pixels with the same label have the same characteristics (like belonging to the same object).

  Image Segmentation has many uses and applications in the field of medical imaging, autonomous vehicles, geographic information systems, etc. 

  ![autonomous car semantic segmentation](nvidia-ss.jpg)
  [Image from nvidia.com](https://blogs.nvidia.com/blog/2016/01/05/eyes-on-the-road-how-autonomous-cars-understand-what-theyre-seeing/)

  Lets look at some of the commonly used algorithms for image segmentation.

  ## Threshold Segmentation

  Threshold segmentation is a simple segmentation method, which divides an image (usually grayscale) into two segments : background and target object. In Thresholding, pixels of an image are partitioned into segments based on their intensity. In a grayscale image, the intensity of a pixel is represented by a number between 0 and 255. We choose a threshold value, and the pixels below the threshold are assigned a value of 1 (white) and pixels above the threshold are assigned a value of 0. This creates two segments, white background whose intensity is 1 and 

  //add formula here

  The method used above is called global thresholding where we use the same threshold value throughout the image, in local thresholding we change the threshold value depending on the intensities of the neighbouring pixel.

  Let's apply thresholding on the following image. The goal is to perform segmentation such that we can extract the apple from the background.

  ![img](apple.jpg)

  Image from unsplash.

  Use [this interactive notebook](--------colablink-----------) to run the following code.

  ```python
  import cv2
  image = cv2.imread("apple.jpg", 0) #reading the image as a grayscale image.
  ret, low = cv2.threshold(img,85,255,cv2.THRESH_BINARY)
  ret, medium = cv2.threshold(img,155,255,cv2.THRESH_BINARY)
  ret, high = cv2.threshold(img,215,255,cv2.THRESH_BINARY)
  #plt.subplots
  #plt.show
  ```
  Grey Image

  ![img]()

  Low threshold

  ![img]()

  Medium Threshold

  ![img]()

  High Threshold

  ![img]()

  0 is black and 255 is white. In `cv2.threshold(image, threshold, max, type)` we choose `THRESH_BINARY`, in the target image it assigns 255 if intensity of pixel is greater than threshold, else assigns 0. We can see that if we take threshold too high, we cannot clearly separate between object and background. If we take threshold too low then some features of the object are not visible. To find the "perfect" threshold, there are many algorithms, lets look at **Otsu's Method**.

  ### Otsu's Method

  Otsu's method is used to find the optimal value for the global threshold. The algorithm searches all the possibilities for a threshold which minimises the weighted sum of variances of the two classes:

  ![formula](otsu_1.jpg)

  ```python
  import cv2
  import matplotlib.pyplot as plt

  img = cv2.imread("apple.jpg", 0)
  ret,thresh1 = cv2.threshold(img,0,255,cv2.THRESH_BINARY+cv2.THRESH_OTSU)
  plt.imshow(thresh1,'gray')
  plt.show()
  ```

  ![otsu](apple_otsu.jpg)

  If we look at the binarised image, it does an almost perfect job. The limitation with otsu's method is it needs a bimodal image for doing a perfect job of segmentation. A bimodal image is, when you draw the histogram of intensities of pixels then you should get two peaks. If you get two peaks, then the midpoint of those is a good threshold. 

## Edge Detection for Image Segmentation

An Edge is a boundary between two homogenous regions. Edge detection is a process of finding boundaries of objects within an image. The boundaries are characterized by immediate change in the intensity of pixels. We use the discontinuities in the image to get image segments. Classical edge detection algorithms involve [Convolving](https://en.wikipedia.org/wiki/Convolution) an image with an operator. An operator is a two dimensional matrix, which is constructed such that it is aware of large gradients in the image but returns zero for uniform regions. Operators can be optimised to look for edges in the vertical, horizontal and diagonal orientation. 

### Sobel Edge Detection

In Sobel method, we calculate the gradient in the horizontal direction and vertical direction; and then we can get the approximation of gradient at a point by combining vertical and horizontal gradient. 

![xaxis](sobel_x.png)

![yaxis](sobel_y.png)

![total](sobel_add.png)

```python
import cv2
img = cv2.imread("image.jpg",0)
scale = 1
delta = 0
ddepth = cv2.CV_16S

grad_x = cv2.Sobel(img, ddepth, 1, 0, ksize=3, scale=scale, delta=delta, borderType=cv2.BORDER_DEFAULT)
grad_y = cv2.Sobel(img, ddepth,0,1,ksize=3, scale=scale, delta=delta, borderType=cv2.BORDER_DEFAULT)

abs_grad_x = cv2.convertScaleAbs(grad_x)
abs_grad_y = cv2.convertScaleAbs(grad_y)

grad = cv2.addWeighted(abs_grad_x, 0.5, abs_grad_y, 0.5, 0)
plt.imshow(grad,'gray')
plt.show()
```

![sobel_img](sobel_orig.png)

![gradient](sobel_grad.png)


### Canny Edge Detection

Canny edge detection is a popular and efficient edge detection algorithm. The algorithm for Canny :

1. Noise Reduction. Edge detection is a difficult task in noisy images, we can reduce the noise in an image by convolving the image with a Gaussian filter.
2. Calculating Gradient Magnitude and Direction. We calculate the gradient of intensity using the same operations as the Sobel method above. We also need to calculate the direction for each pixel. 

$Angle(\theta) = \arctan(G_y / G_x)$

3. Non Maximal Supression. Pixels which are not edge pixels are removed from the image. This is done by checking if a pixel is the local maxima in its neighbourhood in the direction of the gradient. We get finer/thin edges from this step.

4. Thresholding. We use two threshold values, minVal and maxVal. Any edges with intensity gradient more than maxVal are sure to be edges and those below minVal are sure to be non-edges, so discarded. Those who lie between these two thresholds are classified edges or non-edges based on their connectivity. If they are connected to “sure-edge” pixels, they are considered to be part of edges. Otherwise, they are also discarded.

```
import cv2

```

![img]()

image segmentation using  

## Segmentation using clustering

## Image Segmentation using machine learning


Image segmentation is an important step in image analysis, the results of processes like image classification depend on how good image segmentation is done. Image Segmentation is used in a wide variety of applications like Aeriel Image Analysis, Land use calculation, Cancer research, etc. Only a few methods haave been discussed in this article, for the latest reasearch in this field please check out . 
## References 
 - http://homes.di.unimi.it/ferrari/ImgProc2011_12/EI2011_12_16_segmentation_double.pdf
 - https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_thresholding/py_thresholding.html
 - http://airccse.org/journal/jcsit/1211csit20.pdf
 - https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_canny/py_canny.html
