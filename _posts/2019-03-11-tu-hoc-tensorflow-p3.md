---
layout: post
title: Tự học Tensorflow [P3] - Dataset, Layer
date: 2019-03-11 10:56:20 +0700
description:  tu hoc tensorflow, tensorflow layer
img: 2019-03-11-tu-hoc-tensorflow-p3/tensorflow.jpg # Add image post (optional)
image-dir: /assets/img/2019-03-11-tu-hoc-tensorflow-p3
fig-caption: # Add figcaption (optional)
category: [Tensorflow]
tags: [Self Learning Tensorflow]
permalink: /2019/03/11/tu-hoc-tensorflow-p3
---
Ở phần trước [Tự học Tensorflow [P2] - Session, Feeding]({{site.url}}/2019/02/26/tu-hoc-tensorflow-p2), mình đã nhắc tới 2 thành phần cơ bản của **Tensorflow** đó là **`tf.Session`** và **`Feeding`**. Trong phạm vi bài viết này, mình xin đề cập tới 2 nội dung quan trọng khi training một models với Tensorflow. Nếu chưa theo dõi nhưng phần trước thì bạn có thể tham khảo thêm:
* [1.Tự học Tensorflow [P1] - Tensor, Graph, TensorBoard]({{site.url}}/2019/02/15/tu-hoc-tensorflow-p1)
* [2.Tự học Tensorflow [P2] - Session, Feeding]({{site.url}}/2019/02/26/tu-hoc-tensorflow-p2)

* TOC
{:toc}

## Dataset
Ở bài trước, chúng ta đã biết **`tf.placeholder`** hoạt động giống như một parameter đầu vào của một phương thức. Vậy khi muốn nạp dữ liệu vào theo một luồng dữ liệu thì thứ chúng ta cần là gì? Câu trả lời chính là **`tf.data`**, nó cung cấp một phương thức cho phép truyền một dòng dữ liệu vào trong model.

<p align="center"><img src="{{page.image-dir}}/dataset.png"/></p>
Để chuyển dữ liệu đầu vào thành một **`tf.Tensor`** chúng ta cần chuyển nó thành dạng **`tf.data.Iterator`** trước tiên. Sau đó sử dụng phương thức **`tf.data.Iterator.get_next`**.
Cách đơn giản nhất để tạo một `Iterator` là dùng phương thức **`tf.data.Dataset.make_one_shot_iterator`**

```python
my_data = [
    [0, 1,],
    [2, 3,],
    [4, 5,],
    [6, 7,],
]
slices = tf.data.Dataset.from_tensor_slices(my_data)
next_item = slices.make_one_shot_iterator().get_next()
```

Ở đây, tensor `next_item` sẽ trả lại giá trị một dòng từ `my_data` mỗi khi được `run`.

```python
print(sess.run(next_item))
```
`Output`:
```python
[0, 1]
```

Khi các giá trị của `Iterator` được lấy hết, `Dataset` sẽ ném ra một exception **`tf.errors.OutOfRangeError`**.
```python
while True:
  try:
    print(sess.run(next_item))
  except tf.errors.OutOfRangeError:
    break
```
Nếu **`Dataset`** được xây dựng lên từ một phương thức của Tensor, chúng ta cần khởi tạo Iterator trước khi sử dụng, giống như việc gọi `sess.run` để Tensor trả về giá trị vậy. Chúng ta sử dụng **`tf.data.Dataset.make_initializable_iterator()`** để khởi tạo chúng.
```python
r = tf.random_normal([10,3])
dataset = tf.data.Dataset.from_tensor_slices(r)
iterator = dataset.make_initializable_iterator()
next_row = iterator.get_next()

sess.run(iterator.initializer)
while True:
  try:
    print(sess.run(next_row))
  except tf.errors.OutOfRangeError:
    break
```
## Layers
Để training một model, giá trị của các parameter trong Graph cần được xác định bằng cách sử dụng **`tf.layers`**. Các Layer sẽ đóng gói các biến và phương thức hoạt động trên nó. Điển hình có thể nói đến đó là fully-connected layers cùng với hàm kích hoạt. Các kết nối giữa các `weights` và `biases` được quản lý bởi đối tượng Layer.
```python
x = tf.placeholder(tf.float32, shape=[None, 3])
linear_model = tf.layers.Dense(units=1)
y = linear_model(x)
```
### Khởi tạo layer
Nếu `Layer` chứa biến thì cần phải được khởi tạo trước khi sửa dụng. Mặc dù có thể khởi tạo các biến một cách độc lập, `Tensorflow` cung cấp phương thức để có thể dễ dàng khởi tạo tất cả các biến có mặt trong Graph:
```python
init = tf.global_variables_initializer()
sess.run(init)
```

`Chú ý`: **global__variables_initializer()** chỉ khởi tạo giá trị cho các biến đã xuất hiện trong Graph khi thực thi khởi tạo. Vậy nên hãy luôn đặt khởi tạo ở cuối cùng của Graph.
### Thực thi Layer
Lúc này Layer đã được khởi tạo, chúng ta có thể bắt đầu khởi chạy một model `Linear` đơn giản.
```python
print(sess.run(y, {x: [[1, 2, 3],[4, 5, 6]]}))
```
`Output`:
```python
[[-3.41378999]
 [-9.14999008]]
```
Giá trị trả về là weight và bias của Layer `Dense` mà chúng ta đã tạo ở trên với số lượng `unint` bằng 1 vậy nên chỉ có 1 weight khi thực thi `fully-connected`.
### Lối tắt của phương thức Layer
Với mỗi lớp Layer (VD: **`tf.layers.Dense`**), Tensorflow cũng hỗ trợ lối tắt (VD: **`tf.layers.dense`**).
Cách sử dụng:
```python
x = tf.placeholder(tf.float32, shape=[None, 3])
linear_model = tf.layers.Dense(units=1)
y = linear_model(x)
```
Chuyển thành:
```python
x = tf.placeholder(tf.float32, shape=[None, 3])
y = tf.layers.dense(x, units=1)
```
`Chú ý`: Tuy cách sử dụng này khá tiện lợi nhưng vì sử dụng lối tắt nên lúc này bạn không thể can thiệp vào layer `Dense` để debug thông qua biến `linear_model` như bình thường vì lúc này vai trò của biến `linear_model` đã biến mất và thay thế bằng cách gọi trực tiếp hàm `dense`.

**Bài viết tham khảo và sử dụng hình ảnh từ:**
* [Low Level APIs from Tensorflow](https://www.tensorflow.org/guide/low_level_intro)