---
layout: post
title: Tìm hiểu về Softmax Activation Function
date: 2019-07-06 00:16:20 +0700
description:  tim hieu ve softmax activation, ham softmax la gi, ham kich hoat softmax, tai sao su dung ham softmax
img: 2019-07-06-ham-softmax/neural_network.png # Add image post (optional)
image-dir: /assets/img/2019-07-06-ham-softmax
fig-caption: # Add figcaption (optional)
category: jekyll
tags: [Mathematic]
permalink: /2019/07/06/ham-softmax
---
Trong những bài toán về phân lớp (classification), nhìn vào cấu trúc mạng chúng ta thường thấy sự xuất hiện của hàm **Softmax** ở những layer cuối. Hay như trong bài viết trước [NLP [P5] - Tìm hiểu về Attention Mechanism]({{site.url}}/2019/07/05/ky-thuat-attention) mình cũng có đề cập tới việc sử dụng hàm Softmax để tính được trọng số. Vậy hàm Softmax là gì? Tại sao lại sử dụng nó?

* TOC
{:toc}

## Exponential Function

Việc đầu tiên cần bàn tới là hàm **Exponential** - Hàm Mũ.

<p align="center"><img alt="Exponential Function" src="{{page.image-dir}}/exp.gif"/></p>

Hàm mũ được định nghĩa: 

$$y=a^x$$ 

với $a$ là một số dương khác 1.
Đặc trưng của nó là luôn trả về một kết quả dương, và là một hàm đồng biến. Với giá trị của x càng lớn thì y càng lớn và ngược lại. Nhìn chung, đồ thị của hàm mũ khá *mượt*. Trong một số trường hợp, $a$ thường được gán với giá trị là $e$ - Cơ số của logarit tự nhiên.

## Softmax Function

Xét lại bài toán phân loại, kết quả đầu ra cần dự đoán được dữ liệu đầu vào thuộc lớp nào, kết quả gồm 100% được chia đều cho tất cả các lớp, lớp nào có xác suất lớn nhất chính là kết quả đầu ra.
Ví dụ:

Mèo - 0.2 -> 20%

Gà - 0.25 -> 25%

Voi - 0.55 -> 55%

Vậy yêu cầu là tìm một hàm nào đó trả về xác suất cho tín hiệu thuộc lớp nào và tổng các xác suất bằng 100%. Một vấn đề gặp phải nữa đó chính là tín hiệu từ các lớp của **Neural Network** có thể là các giá trị âm. Vậy ta cần một hàm số *mượt* luôn trả về giá trị dương để dễ dàng cho quá trình tính toán. Thêm vào đó, tín hiệu của neural càng mạnh thì xác suất thuộc lớp đó càng lớn, vì vậy ta cần một hàm đồng biến để có thể đánh giá chính xác giá trị của neural đó.
Quay trở lại hàm Mũ nêu trên, ta thấy được hàm mũ đảm bảo được giá trị trả về luôn dương, đồng biến. Để đáp ứng được việc tổng bằng 1, ta chỉ cần lấy tỉ lệ của mỗi tham số với tổng của tất cả các tham số.

$$a_i=\frac{\exp\left(z_i\right)}{\sum_{j-1}^C \exp\left(z_j\right)}$$

$C$ là số lượng các lớp cần được phân loại. Hàm số này được gọi là hàm Softmax. Một điểm cần lưu ý là không có giá trị nào của hàm Softmax có giá trị là 0 hoặc 1 mặc dù rất gần với 0, 1. Trong trường hợp các tham số quá lớn, có thể dẫn tới tràn số khi đưa qua hàm Mũ, ta có thể trừ chúng đi một lượng $\alpha$ nhất định.

$$a_i=\frac{\exp\left(z_i-\alpha\right)}{\sum_{j-1}^C \exp\left(z_j-\alpha\right)}$$

<p align="center"><img alt="Exponential Function" src="{{page.image-dir}}/softmax.png"/></p>

Vì những đặc điểm trên, hàm Softmax thường được sử dụng ở những layer cuối của mạng Classification nhằm đánh giá xác suất phân loại của dữ liệu đầu vào. Ngoài ra, hàm Softmax cũng thường được sử dụng để tính toán trọng số cho dữ liệu.

**Bài viết được tham khảo và sử dụng hình ảnh từ**:
* [Applied Deep Learning - Part 1: Artificial Neural Networks](https://towardsdatascience.com/applied-deep-learning-part-1-artificial-neural-networks-d7834f67a4f6)
* [Exponential Function](http://mathworld.wolfram.com/ExponentialFunction.html)
* [Softmax Activation Function](http://rinterested.github.io/statistics/softmax.html)