---
layout: post
title: Con đường học AI
date: 2019-09-30 00:16:20 +0700
description: hoc ai can gi, con duong hoc ai, hoc deep learning nhu the nao, cach hoc deep learning, cach hoc machine learning
img: 2019-09-30-con-duong-hoc-ai/Deep_Learning_Icons_R5_PNG.png # Add image post (optional)
image-dir: /assets/img/2019-09-30-con-duong-hoc-ai
fig-caption: # Add figcaption (optional)
category: [Computer Vision, Ultilities]
tags: [Machine Learning, Deep Learning]
permalink: /2019/09/30/con-duong-hoc-ai
---

Bài viết này được tổng hợp từ kinh nghiệm và trải nghiệm của chính tác giả, chỉ  nên dùng để tham khảo cho những bạn đang tìm lộ trình học tập cho chính bản thân mình về mảng AI - Trí tuệ nhân tạo hay Computer Vision. Nếu bạn đọc có hứng thú về Computer Vision đặc biệt là Digital Image Processing thì có thể tham khảo bài viết về [Xử Lí Ảnh]({{site.url}}/) của mình.

* TOC
{:toc}

## Kiến thức toán cần thiết

Nếu bạn là một người học chăm chỉ những môn như Toán rời rạc, Đại số, Giải tích thì có thể bỏ qua phần này, nhưng nếu bạn là một người thường bỏ qua hay học hời hợt những bộ môn này khi còn học đại học, thì điều đó có thể sẽ làm chậm quá trình học của bạn. Việc tiếp xúc với Paper hay đọc những công thức hàm **Loss** tuy không quá phức tạp nhưng những kí hiệu toán học có thể sẽ khiến bạn bối rối khi học như:

$\sum$

$\sum_{i=0}^\infty$

$\prod$

$\int$

Ngoài ra, một số kiến thức như nhân ma trận, ma trận chuyển vị, phương sai, độ lệch chuẩn, phân bố xác suất là những khái niệm bạn cần nắm vững trước khi bắt đầu học. Kinh nghiệm của mình là đừng đi quá nhanh, hãy đi từ từ, chắc chắn, nắm rõ những khái niệm cơ bản trước khi bắt đầu tiến tới những bài giảng về Machine Learning. Bạn đọc có thể tham khảo thêm các kiến thức về toán dùng trong Machine Learning tại [đây](https://github.com/tiepvupsu/tiepvupsu.github.io/blob/master/Math_ML.pdf)

## Machine Learning cơ bản

Sau khi đã nắm rõ những khái niệm toán học, bạn có thể bắt đầu với những bài toán, khái niệm cơ bản của Machine Learning. Bạn đọc có thể tìm hiểu từ nguồn blog [Machinelearningcoban](https://machinelearningcoban.com/) của tác gỉa *Tiep Huu Vu*. Từ series bài 1 cho tới 14 có thể giúp bạn nắm được những khái niệm, thuật toán thường dùng trong Machine Learning, có thể tiến tới học về Deep Learning. Một số những khái niệm cần nắm rõ đó là:
* Linear Regression
* Gradient Descent
* Backpropagation
* Perceptron

<p align="center"><img alt="Machinelearningcoban.com" src="{{page.image-dir}}/pic1.png"/></p>

Cũng trong những bài viết này, bạn có thể nắm rõ các khái niệm về **hàm Loss** - cốt lõi chính của Machine Learning là tìm cách tối thiêu hàm Loss, ngoài ra một số khái niệm khác như Activation Function, Classification cũng cần được nắm chắc.

## Deep Learning cơ bản

Sau khi đã nắm rõ được những khái niệm, kĩ thuật căn bản của MachineLearning, bạn có thể tiếp tục làm quen với những bài toán của Deep Learning về Computer Vision như Classification, Segmentation. Bạn đọc có thể tham khảo khóa học của **Stanford** về mạng CNN - [CS231n: Convolutional Neural Networks for Visual Recognition](http://cs231n.stanford.edu/). Song song với việc học tập thì việc thực hành cũng vô cùng quan trọng. Bạn đọc có thể làm quen với những bài toán như Phân biệt chó mèo, phân biệt kí tự số, ... từ các cuộc thi tìm hiểu nhỏ trên [Kaggle](https://www.kaggle.com/competitions) đây là một trang thường tổ chức các cuộc thi để tìm hiểu và có giải thưởng về Deep Learning. Trang cũng cung cấp data sẵn có về các bài toán này giúp cho người dùng với mục đích học tập có thể dễ dàng có được data để sử dụng.

<p align="center"><img alt="Kaggle" src="{{page.image-dir}}/pic2.png"/></p>

Ngoài ra, một số khóa học khác mà bạn đọc có thể tham khảo thêm:
* [Stanford CS230: Deep Learning](https://www.youtube.com/watch?v=PySo_6S4ZAg&list=PLoROMvodv4rOABXSygHTsbvUz4G_YQhOb&index=1)
* [Natural Language Processing with Deep Learning](https://www.youtube.com/watch?v=PySo_6S4ZAg&list=PLoROMvodv4rOABXSygHTsbvUz4G_YQhOb&index=1)

## Machine Learning Framework

<p align="center"><img alt="Keras" src="{{page.image-dir}}/pic3.png"/></p>

Khi mới tiếp cận với việc training các model đơn giản, thì bạn đọc nên tiếp cận với những High Level Framework như [Keras](https://www.tensorflow.org/guide/keras/overview) bởi sự cung cấp các phương tiện để xây dựng một Network nhanh nhất có thể, tuy nhiên khi muốn tùy chỉnh nhiều hơn các tham số, cấu trúc, thì nên tìm hiểu về Tensorflow. Bởi hầu hết những cấu trúc lớn, phức tạp đều được deploy bằng [Tensorflow](https://www.tensorflow.org/guide). Ngoài ra, cũng có một số Framework khác mà bạn đọc có thể tham khảo:

* [Pytorch](https://pytorch.org/)
* [Caffee](https://caffe.berkeleyvision.org/)

<p align="center"><img alt="Tensorflow" src="{{page.image-dir}}/pic4.png"/></p>



Bài viết được tham khảo và sử dụng tài liệu từ:
* [What’s the Difference Between Artificial Intelligence, Machine Learning, and Deep Learning?](https://blogs.nvidia.com/blog/2016/07/29/whats-difference-artificial-intelligence-machine-learning-deep-learning-ai/)
* [10 Tips to Get Started with Kaggle](https://medium.com/@ODSC/10-tips-to-get-started-with-kaggle-fc7cb9316d27)
* [machinelearningcoban.com](machinelearningcoban.com)
* [Math MachineLearning](https://github.com/tiepvupsu/tiepvupsu.github.io/blob/master/Math_ML.pdf)