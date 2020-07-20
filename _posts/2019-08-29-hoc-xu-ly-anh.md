---
layout: post
title: Học Xử Lý Ảnh Số - Bắt đầu từ đâu?
date: 2019-08-29 00:16:20 +0700
description: hoc xu ly anh, xu ly anh la gi, học xử lý ảnh, hoc xu li anh, xu li anh, xử lý ảnh là gì, computer vision, opencv la gi, digital image processing, xu ly anh so, xử lý ảnh số
img: 2019-08-29-hoc-xu-ly-anh/opencv.png # Add image post (optional)
image-dir: /assets/img/2019-08-29-hoc-xu-ly-anh
fig-caption: # Add figcaption (optional)
category: [Computer Vision]
tags: [Digital Image Processing]
permalink: /2019/08/29/hoc-xu-ly-anh
---
Trong bài viết này, tác giả sẽ cung cấp những kiến thức cần thiết cho những người có nhu cầu tìm hiểu về Xử lý ảnh số (viết tắt XLA) hoặc những người tò mò muốn biết Xử lí ảnh số gồm những bài toán nào, làm gì. Bài viết được tham khảo và tổng kết từ series [OpenCV-Python Tutorials](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_tutorials.html) của **OpenCV**. Ngôn ngữ sử dụng chính trong bài viết là *Python*

* TOC
{:toc}

## OpenCV là gì?
OpenCV là viết tắt của Open Computer Vision, như tên gọi, nó là một thư viện mã nguồn mở dành cho Computer Vision - Thị giác máy tính. OpenCV được xây dựng bằng C/C++ và tích hợp trực tiếp với package [Numpy](https://numpy.org/) nên việc xử lý có tốc độ rất nhanh, đáp ứng những ứng dụng yêu cầu thời gian thực. Ngoài ra, OpenCV cũng có thể chạy trên những nền tảng khác nhau như Linux, Window, Mac cũng như những ngôn ngữ khác nhau như Java, Python, C++, ...

## Học những gì?
Dưới đây là một số những kiến thức mà bản thân tác giả cho là cần thiết khi tiếp cận với mảng Xử lý ảnh, chỉ mang tính chất tham khảo.

### Không gian màu
Để bắt đầu với XLA thì điều đầu tiên cần tìm hiểu đó chính là Không gian màu, có lẽ bạn đọc đã quá thân thuộc với từ `RGB` - Red, Green, Blue ở bậc trung học, đây là ba màu cơ bản được định nghĩa cho hệ màu RGB. Ngoài ra, còn một số hệ màu khác, phục vụ cho những mục đích khác nhau như HSV, HSL, Gray, ... Bạn đọc cần nắm rõ các biểu diễn ở mỗi hệ màu và công thức chuyển đổi giữa các hệ màu đó.

<p align="center"><img alt="Color Spaces" src="{{page.image-dir}}/pic1.png"/></p>

Bạn đọc có thể tham khảo thêm [Cách thức chuyển đổi giữa các hệ màu](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_colorspaces/py_colorspaces.html#converting-colorspaces)

### Xử lí độ sáng và tương phản
Bức ảnh thu được có thể có độ sáng quá thấp, hoặc quá cao đặc biệt trong môi trường ngoài trời thì có thể sẽ có những vùng có độ sáng khác nhau. Vì vậy, kiến thức về độ sáng và tương phản cũng vô cùng cần thiết.

<p align="center"><img alt="Brightness and Constrast" src="{{page.image-dir}}/pic2.png"/></p>


### Histogram
Histogram là khái niệm thể hiện tần suất xuất hiện của các giá trị của một pixel trong bức ảnh.

<p align="center"><img alt="Histogram" src="{{page.image-dir}}/pic3.png"/></p>

<p align="center"><img alt="Histogram" src="{{page.image-dir}}/pic4.png"/></p>

Việc cân bằng Histogram (Histogram equalization) là một bước tiền xử lý ảnh đầu vào, cải thiện chất lượng bước ảnh.
Bức ảnh sau khi được cân bằng histogram:
<p align="center"><img alt="Histogram equalization" src="{{page.image-dir}}/pic5.png"/></p>

### Binary Image - Kỹ thuật xử lý ảnh nhị phân
Việc xử lý bức ảnh trở thành ảnh nhị phân thường được dùng trong những bài toán như nhận diện vật thể (Object Detection) hay phân chia (Segmentation). Việc này giúp đánh bật những vật thể mong muốn ra khỏi nền của bức ảnh, giúp xác định vị trí của vật thể mong muốn. Tùy vào mục địch hoặc đặc tính của bức ảnh mà ta có những phương pháp nhị phân khác nhau như:
* Phân tích ngưỡng cố định: Simple Threshold
* Phân tích ngưỡng động: Adaptive Threshold, Otsu Threshold.

<p align="center"><img alt="Threshold" src="{{page.image-dir}}/pic6.png"/></p>

Bạn đọc có thể tham khảo thêm [Image Thresholding](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_thresholding/py_thresholding.html#thresholding)
Song hành cùng Thresholding là một số kỹ thuật xử lí ảnh Binary gọi là Morphological Transform. Bao gồm những phép đóng, mở ảnh, hỗ trợ cho việc tìm đường bao về sau:

<p align="center">
    <img alt="Sample" src="{{page.image-dir}}/sample.png"/>
    <img alt="Close" src="{{page.image-dir}}/close.png"/>
    <img alt="Open" src="{{page.image-dir}}/open.png"/>
    <img alt="Morphological Gradient" src="{{page.image-dir}}/mopho_gradient.png"/>
</p>

Tham khảo [Morphological Transformations](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_morphological_ops/py_morphological_ops.html#morphological-ops)

### Geometric Transform - Kỹ thuật biến đổi hình học

Ngoài vấn đề về độ sáng, tương phản, thì ta còn gặp những trường hợp bức ảnh bị biến dạng, nằm nghiêng, kích cỡ lớn. Dưới đây là một số những kỹ thuật thường sử dụng khi gặp những trường hợp này:

* Scaling - Biến đổi về kích thước 
* Rotating - Xoay ảnh
* Translation - Dịch chuyển bức ảnh theo các hướng khác nhau
* Perspective Transform, Affine Transform - Biến đổi bức ảnh theo các điểm mốc

<p align="center">Translation</p>
<p align="center"><img alt="Translation" src="{{page.image-dir}}/translation.jpg"/></p>
<p align="center">Rotation</p>
<p align="center"><img alt="Rotation" src="{{page.image-dir}}/rotation.jpg"/></p>
<p align="center">Affine</p>
<p align="center"><img alt="Affine" src="{{page.image-dir}}/affine.jpg"/></p>
<p align="center">Perspective</p>
<p align="center"><img alt="Perspective" src="{{page.image-dir}}/perspective.jpg"/></p>

Tham khảo thêm [Geometric Transformations of Images](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_geometric_transformations/py_geometric_transformations.html#geometric-transformations)

### Filter - Lọc ảnh

<p align="center"><img alt="Laplace Filter" src="{{page.image-dir}}/filter_all.png"/></p>

Việc sử dụng lọc ảnh có thể giúp ta loại bỏ những loại nhiễu trong bức ảnh, tăng cường chất lượng cho bức ảnh hoặc biến đổi bức ảnh theo hướng ta mong muốn. Đặc thù của lọc ảnh là sử dụng những filter/kernel lên bức ảnh dưới dạng các *2D Convolution*. Một số filter đặc trưng có thể kể đến như Gaussian Blurring, Median Blurring, ...

<p align="center">Blur</p>
<p align="center"><img alt="Blur" src="{{page.image-dir}}/blur.jpg"/></p>
<p align="center">Median Blur</p>
<p align="center"><img alt="Median Blur" src="{{page.image-dir}}/median.jpg"/></p>
<p align="center">Sobel Filter</p>
<p align="center"><img alt="Sobel Filter" src="{{page.image-dir}}/sobel.png"/></p>
<p align="center">Laplace Filter</p>
<p align="center"><img alt="Laplace Filter" src="{{page.image-dir}}/laplacian.png"/></p>

Tham khảo [Smoothing Images](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_filtering/py_filtering.html#filtering)

### Biến đổi Hough, kỹ thuật phát hiện biên

Việc sử dụng biến đổi *Hough* có thể giúp ta phát hiện được những đường thẳng, đường tròn. Ngoài ra còn có một số kĩ thuật giúp phát hiện các đường biên, nét cạnh của vật thể như *Canny*.

<p align="center">Edge Detection</p>
<p align="center"><img alt="Canny" src="{{page.image-dir}}/canny1.jpg"/></p>
<p align="center"><img alt="Canny" src="{{page.image-dir}}/canny.png"/><img alt="Canny" src="{{page.image-dir}}/canny_line.png"/></p>
<p align="center">Hough Circle and Line</p>
<p align="center"><img alt="HoughCircle" src="{{page.image-dir}}/hough_circle.png"/></p>

Tham khảo [Canny Edge Detection](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_canny/py_canny.html#canny)

Ngoài ra, một số phương pháp phát hiện *Contour* cũng được sử dụng để phát hiện vật thể dựa trên những biên ảnh [Contours in OpenCV](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_contours/py_table_of_contents_contours/py_table_of_contents_contours.html#table-of-content-contours)

## Học như thế nào?

Trong quá trình học lý thuyết, chúng ta cần những bài tập thực hành để vận dụng những kiến thức đã học. Bạn đọc nên bắt đầu từ những bài toán có thể vận dụng một hoặc vài kỹ thuật đã học như Phát hiện vật thể bằng Threshold, nhận dạng biển số và phân chia chữ số của biển, phân chia kí tự trên một dòng chữ, vv...vv

<p align="center">Segment Number</p>
<p align="center">
    <img alt="Segmentation number" src="{{page.image-dir}}/license_gray.jpg"/>
    <img alt="Segmentation number" src="{{page.image-dir}}/license_thresh.jpg"/>
    <img alt="Segmentation number" src="{{page.image-dir}}/license_segment.jpg"/>
</p>

<p align="center">Card Detect</p>
<p align="center">
    <img alt="Segmentation number" src="{{page.image-dir}}/id_card_gray.jpg"/>
    <img alt="Segmentation number" src="{{page.image-dir}}/id_card_segment.jpg"/>
</p>


Bài viết được tham khảo và sử dụng tài liệu từ:
* Ứng dụng xử lý ảnh trong thực tế với thư viện OpenCV - Nguyễn Văn Long (long.06clc@gmail.com)
* [OpenCV Tutorials](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_tutorials.html)