---
layout: post
title: Tự học Tensorflow [P1] - Tensor, Graph, TensorBoard
date: 2019-02-15 08:56:20 +0700
description:  tu hoc tensorflow, tensor la gi, cach su dung tensorboard
img: 2019-02-15-tu-hoc-tensorflow-p1/tensorflow.jpg # Add image post (optional)
image-dir: /assets/img/2019-02-15-tu-hoc-tensorflow-p1
fig-caption: # Add figcaption (optional)
category: [Tensorflow]
tags: [Self Learning Tensorflow]
permalink: /2019/02/15/tu-hoc-tensorflow-p1
---
Nhằm đáp ứng nhu cầu công việc của team AI, nên gần đây mình cần tìm hiểu về **Tensorflow**. Trong chuỗi blog này mình xin chia sẻ về những kiến thức mình đã tìm hiểu được về **Tensorflow**, vừa là hệ thống lại kiến thức đồng thời giúp những thành viên khác trong team có thể tiếp cận với **Tensorlfow** tốt hơn. Trong bài viết nếu có phần nào không đúng mong mọi người góp ý ở cuối bài viết :D

* TOC
{:toc}

## [Tensorflow](https://vi.wikipedia.org/wiki/TensorFlow) là gì?

Nói một cách dễ hiểu, **Tensorflow** là một thư viện mã nguồn mở dùng để xử lí cách phép tính toán số học bằng cách mô tả một mô hình biểu đồ thể hiện sự thay đổi về giá trị của dữ liệu. Tiền thân của **Tensorflow** là **DistBelief** - dự án về hệ thống học máy của [Google Brain](https://en.wikipedia.org/wiki/Google_Brain) được phát triển vào năm 2011. **Tensorflow** là dự án thứ 2, được phát hành dưới dạng mã nguồn mở vào 09/11/2015.

## [Cài đặt](https://www.tensorflow.org/install)

Bạn có thể cài đặt **tensorflow** bằng cách dùng `pip` hoặc `conda`:

```
pip install tensorflow
```

Hiện tại **tensorflow** đã có phiên bản `1.13(stable)` và `2.00(preview)`
 
## Tensor

Đơn vị dữ liệu chính của **Tensorflow** là những **Tensor** và **Tensorflow** là dòng chảy của những **Tensor**.
Một Tensor bao gồm một tập hợp các giá trị nguyên thủy (integer, float, string, ..) cấu thành nên một tập hợp không giới hạn số chiều. 
* **Rank** của một Tensor là số chiều của nó
* **Shape** là một bộ (tuple) của các số biểu diễn số lượng phần tử có trong mỗi chiều.
<br>
Ví dụ:
<br>
`3.0` : Tensor với rank là **0**, nói cách khác là 1 `Scalar`
<br>
`[1., 2., 3.]`: Tensor với rank là **1**, là một vector có shape là `[3]`
<br>
`[[1., 2., 3.], [4., 5., 6.]]`: Tensor với rank là **2**, là một ma trận có shape là `[2, 3]`
<br>
`[[[1., 2., 3.]], [[7., 8., 9.]]]`: Tensor với rank là **3** với shape là `[2, 1, 3]`

`Lưu ý`: Tensorflow sử dụng mảng [numpy](http://www.numpy.org/) để biểu diễn các giá trị của Tensor.

## Các thành phần chính của TensorFlow

Nhìn chung, các thành phần chính của một chương trình TensorFlow bao gồm:

* **`tf.Graph`**: Cấu thành nên đồ thị tính toán.
* **`tf.Session`**: Khởi chạy đồ thị tính toán.
Những thành phần khác sẽ có trong các viết tiếp theo, trong phạm vi bài viết này mình sẽ chỉ nhắc tới một số thành phần chính :D.

### Graph - Đồ thị

<p align="center"><img src="{{page.image-dir}}/tensors_flowing.gif"></p>

Graph (Đồ thị) hay một đồ thị tính toán là một tập hợp các phép toán, thao tác của Tensorflow được đặt vào trong một đồ thị. Một graph được cấu thành bởi 2 loại đối tượng chính:
* **`tf.Operation`**:  Là các node của Graph. **Operation** mô tả sự tính toán để tạo ra các tensor.
*  **`tf.Tensor`**: Là các cạnh của Graph. Chúng biểu diễn các giá trị dữ liệu xuyên suốt đồ thị. Hầu hết các hàm của Tensorflow đều trả lại **tf.Tensors**
  
Hãy cùng xây dựng một đồ thị tính toán đơn giản để hiểu rõ hơn về các thành phần. Phương thức đơn giản nhất đó là `tf.constant` nhận đầu vào là giá trị của Tensor. Phương thức trả về không yêu cầu giá trị đầu vào. Khi khởi chạy Graph, nó sẽ trả về giá trị mà được đặt vào trong hàm khởi tạo.

```python
a = tf.constant(3.0, dtype=tf.float32)
b = tf.constant(4.0) # Ở đây b tự nhân kiểu dữ liệu là tf.float32
total = a + b
print(a)
print(b)
print(total)
```
Output:
```
Tensor("Const:0", shape=(), dtype=float32)
Tensor("Const_1:0", shape=(), dtype=float32)
Tensor("add:0", shape=(), dtype=float32)
```

Có thể thấy rằng `print` Tensor không trả lại giá trị `3.0`, `4.0` và `7.0`. Đoạn code trên chỉ xây dựng lên một đồ thị tính toán, nhưng Tensor chỉ thể hiện kết quả của phương thức sẽ chạy.
`Lưu ý`: Mỗi phương thức trong đồ thị có một tên riêng duy nhất. Tên này độc lập với tên của đối tượng được khởi tạo bởi Python. Các Tensor được đặt tên bởi tên phương thức tạo nên chúng cùng với một chỉ số phía sau `"add:0"`.

### TensorBoard

Tensorflow cung cấp một công cụ khá hữu dụng đó là **Tensorboard**. Một trong những tính năng của nó là khả năng biểu diễn lại cấu trúc của đồ thị mà chúng ta đã tạo.
<p align="center"><img src="{{page.image-dir}}/tensorboard-distributions.png"></p>
Đầu tiên, chúng ta lưu đồ thị lại dưới dạng file Tensorboard
```python
writer = tf.summary.FileWriter('.')
writer.add_graph(tf.get_default_graph())
writer.flush()
```

File sẽ được lưu lại dưới dạng một `event` file với format sau:
```
events.out.tfevents.{timestamp}.{hostname}
```

Giờ chúng ta có thể xem lại cấu trúc đồ thị bằng cách chạy câu lệnh trên Terminal tại thư mục chứ file event:

```
tensorboard --logdir=.
```

Graph sẽ được biểu diễn tại page với url được cung cấp dưới dạng `localhost:6060/#graphs`
<p align="center"><img src="{{page.image-dir}}/getting_started_add.png"></p>

Bạn có thể tìm hiểu kĩ hơn về TensorBoard tại [đây](https://www.tensorflow.org/guide/graph_viz)

Như vậy bài viết đã cung cấp một số kiến thức cơ bản về **Tensorflow**. Nhớ like và kipalog bài viết nếu thấy hữu ích nhé :D

**Bài viết được tham khảo và sử dụng hình ảnh từ :**
* [1. 9 Things You Should Know About TensorFlow](https://hackernoon.com/9-things-you-should-know-about-tensorflow-9cf0a05e4995)
* [2. Graph and Session](https://www.tensorflow.org/guide/graphs)
* [3.Introduction to TensorFlow for Developers. Part 2/?: Tensorboard and colab](https://medium.com/12-developer-labors/introduction-to-tensorflow-for-developers-part-2-tensorboard-b0d76f74108c)
* [4. Low Level APIs from Tensorflow](https://www.tensorflow.org/guide/low_level_intro)
