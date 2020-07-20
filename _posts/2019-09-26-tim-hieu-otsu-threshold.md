---
layout: post
title: Tìm hiểu về  Otsu threshold
date: 2019-09-26 00:16:20 +0700
description: thuat toan otsu, otsu là gì, thuật toán otsu, cách hoạt động của otsu, otsu threshold
img: 2019-09-26-tim-hieu-otsu-threshold/Lenna_threhsold.jpg # Add image post (optional)
image-dir: /assets/img/2019-09-26-tim-hieu-otsu-threshold
fig-caption: # Add figcaption (optional)
category: [Computer Vision]
tags: [Digital Image Processing]
permalink: /2019/09/26/otsu-threshold
---
Thresholding - Phân ngưỡng là một trong số những kĩ thuật trong việc xử lí ảnh số. Đây là kĩ thuật tiền đề cho các kĩ thuật khác như Contours, Houghlines, ... Việc sử dụng threshold giúp phân tách 2 đối tượng foreground và background. Trong bài viết này, tác giả xin giới thiệu tới bạn đọc một kĩ thuật Thresholding (Binarization) thường được sử dụng: **Otsu thresholding**.

* TOC
{:toc}

## Đôi điều về Otsu
*Otsu* được lấy tên theo tên tác giả của phương pháp này **Nobuyuki Otsu** đã giới thiệu về kĩ thuật này trong tập báo của [**IEEE Transactions on Systems, Man, and Cybernetics** - **Hiệp hội hệ thống, con người và điều khiển mạng**](https://sci-hub.tw/10.1109/tsmc.1979.4310076) trang **62-66** của **IEEE** (Institute of Electrical and Electronics Engineers - Viện kỹ sư Điện và Điện Tử) - một tổ chức phi lợi nhuận.

## Cách thức hoạt động

Theo như tên gọi bài báo của tác giả **A Threshold Selection Method from Gray-Level Histograms**, phương pháp Otsu tập trung vào việc khai thác và tính toán từ thông tin Histogram của bức ảnh. Bằng việc tính toán trên tất cả các mức Threshold, ta có thể chọn mức thỏa mãn việc phân chia giữa Foreground và Background tốt nhất.
<p align="center"><img alt="Otsu Visualization" src="{{page.image-dir}}/Otsu's_Method_Visualization.gif"/></p>

### Công thức tính toán

Otsu thực hiện tính toán 3 tham số  chính $\omega$, $\mu$, $\sigma$ đại diện cho Trọng số, giá trị trung bình, phương sai. Ví dụ đơn giản với bức ảnh 6x6 với 6 mức xám sau:

<p align="center"><img alt="Example" src="{{page.image-dir}}/pic1.png"/></p>

Lựa chọn ngưỡng ban đầu bằng 3, ta tính toán trên 2 tập Background < 3 và Foreground >= 3 như sau:

#### Background

<p align="center"><img alt="Background" src="{{page.image-dir}}/pic2.png"/></p>

Trọng số Weight  :  $\omega_b = \frac{8+7+2}{36} = 0.4722$

Giá trị trung bình Mean    :  $\mu_b = \frac{\left(0\times8\right) + \left(1\times7\right) + \left(2\times2\right)}{17} = 0.6471$

Phương sai Variance:  $\sigma_b^2 = \frac{\left(\left(0-0.6471\right)^2\times8\right) + \left(\left(1-0.6471\right)^2\times7\right)+\left(\left(2-0.6471\right)^2\times2\right)}{17} = 0.4637$

#### Foreground

<p align="center"><img alt="Background" src="{{page.image-dir}}/pic3.png"/></p>

Trọng số Weight  :  $\omega_f = \frac{6+4+9}{36} = 0.5278$

Giá trị trung bình Mean    :  $\mu_f = \frac{\left(3\times6\right) + \left(4\times9\right) + \left(5\times4\right)}{19} = 3.8947$

Phương sai Variance:  $\sigma_f^2 = \frac{\left(\left(3-3.8947\right)^2\times6\right) + \left(\left(4-3.8947\right)^2\times9\right)+\left(\left(5-3.8947\right)^2\times4\right)}{19} = 0.5152$

#### Within-Class Variance

Cuối cùng, ta tính toán phương sai theo lớp:

$$\sigma_W^2 = \omega_b\sigma_b^2 + \omega_f\sigma_f^2 = 0.4722*0.4637+0.5278*0.5152=0.4909$$

Bằng việc tính toán như trên với tất cả các giá trị ngưỡng có thể trong bức ảnh, ta được một bảng như sau:

<p align="center"><img alt="Within-Class Variance Table" src="{{page.image-dir}}/pic4.png"/></p>

Chọn giá trị có **Within-Class Variance** nhỏ nhất, ta chọn được mức ngưỡng thích hợp là **T=3**.

<p align="center"><img alt="Threshold Result" src="{{page.image-dir}}/pic5.png"/></p>

Ngoài ra, một hướng tiếp cận nữa giúp giảm độ tính toán của phương pháp là tính toán **Between Class Variance** có giá trị lớn nhất.

$$\sigma_B^2 = \sigma^2 - \sigma^2_W$$

$$=W_b\left(\mu_b - \mu\right)^2 + W_f\left(\mu_f-\mu\right)^2$$

$$=W_bW_f\left(\mu_b-\mu_f\right)^2$$

với $$\mu = W_b\mu_b + W_f\mu_f$$

<p align="center"><img alt="Between Class Variance" src="{{page.image-dir}}/pic6.png"/></p>

Bài viết được tham khảo và sử dụng tài liệu từ:
* [Otsu Threshold](http://www.labbookpages.co.uk/software/imgProc/otsuThreshold.html)
* [IEEE Transactions on Systems, Man, and Cybernetics](https://sci-hub.tw/10.1109/tsmc.1979.4310076)