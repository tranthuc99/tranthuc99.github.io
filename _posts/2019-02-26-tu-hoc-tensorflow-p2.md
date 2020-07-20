---
layout: post
title: Tự học Tensorflow [P2] - Session, Feeding
date: 2019-02-26 09:56:20 +0700
description:  tu hoc tensorflow, session la gi
img: 2019-02-26-tu-hoc-tensorflow-p2/tensorflow.jpg # Add image post (optional)
image-dir: /assets/img/2019-02-26-tu-hoc-tensorflow-p2
fig-caption: # Add figcaption (optional)
category: [Tensorflow]
tags: [Self Learning Tensorflow]
permalink: /2019/02/26/tu-hoc-tensorflow-p2
---
Tiếp tục series [Tự học Tensorflow [P1] - Tensor, Graph, TensorBoard]({{site.url}}/2019/02/15/tu-hoc-tensorflow-p1) , hôm nay mình xin chia sẻ tiếp về quá trình học tập của mình. Nếu có gì không đúng mọi người comment ở cuối bài viết nhé :D

* TOC
{:toc}

## Các thành phần chính của Tensorflow

Trong phạm vi bài viết này, mình sẽ đề cập tới những nội dung nằm trong những phần cơ bản của Tensorflow như:
* **`tf.Session`**
*  **`Feeding`**

### Session

Như đã chia sẻ ở phần [Graph]({{site.url}}/2019/02/15/tu-hoc-tensorflow-p1), khi `print` một Tensor sẽ không trả về giá trị của Tensor mà chỉ trả về cấu trúc của Tensor. Việc này giống như một cỗ máy chỉ chạy khi có dòng điện chảy qua, và **Session** ở đây đóng vai trò như công tác đóng mở dòng điện chảy vào Graph.
<p align="center"><img src="{{page.image-dir}}/session.png"/></p>
Nếu coi `tf.Graph` như một file `.py` thì Session chính là lệnh thực thi `python`.
Chúng ta khởi chạy Graph bằng việc gọi đến phương thức `run` của lớp `Session`
```python
# total = a + b
sess = tf.Session()
print(sess.run(total))
```
Biến `total` đã được ta khởi tạo ở [P1]({{site.url}}/2019/02/15/tu-hoc-tensorflow-p1). Kết quả trả về:
```
7.0
```
Khi một Tensor được đặt bên trong phương thức `run`, **Tensorflow** sẽ truy ngược lại xuyên suốt Graph và khởi chạy tất cả các node mà cung cấp đầu vào cho node được yêu cầu. Ở trong ví dụ này khi node `total` được yêu cầu, Tensorflow sẽ truy ngược tới 2 node cung cấp đầu vào chính là node `a` và `b`. Chúng ta cũng có thể đặt nhiều node/Tensor vào trong phương thức `run` bằng `tuple` và `dict`.
```python
print(sess.run({'ab':(a, b), 'total':total}))
```
`Output`:
```
{'total': 7.0, 'ab': (3.0, 4.0)}
```
`Chú ý`: Trong suốt quá trình gọi phương trức `tf.Session.run` thì giá trị của một Tensor là không thay đổi
```python
vec = tf.random_uniform(shape=(3,))
out1 = vec + 1
out2 = vec + 2
print(sess.run(vec))
print(sess.run(vec))
print(sess.run((out1, out2)))
```
`Output`:
```

[ 0.52917576  0.64076328  0.68353939]
[ 0.66192627  0.89126778  0.06254101]
(
  array([ 1.88408756,  1.87149239,  1.84057522], dtype=float32),
  array([ 2.88408756,  2.87149239,  2.84057522], dtype=float32)
)
```
Dễ thấy dù session gọi tới Tensor `vec` 2 lần nhưng giá trị của `vec` vẫn được giữ nguyên vì cả `out1` và `out2` đều được gọi đến trong cùng 1 quá trình của `sess.run`
Một vài phương thức của **Tensorflow** trả lại `tf.Operations` thay vì `tf.Tensor`. Khi gọi `run` tới một `tf.Operations` sẽ trả lại `None` vì phương thức chỉ thực hiện việc tính toán chứ không trả về giá trị.
### Feeding
Có vẻ tới đây, bạn đã cảm thấy hơi nhàm chán với `tf.Graph` với những giá trị bất biến. Vậy khi muốn xử lí Graph với một chuỗi giá trị đầu vào thì cần làm gì, giải pháp của chúng ta là `tf.placeholder`
```python
x = tf.placeholder(tf.float32)
y = tf.placeholder(tf.float32)
z = x + y
```
2 Tensor `x` và `y` đóng vai trò như 2 arguments đầu vào của một phương thức. Khi gọi tới `tf.Session.run` chúng ta cần thêm `feed_dict` để truyền tham số vào cho `tf.placeholder`
```python
print(sess.run(z, feed_dict={x: 3, y: 4.5}))
print(sess.run(z, feed_dict={x: [1, 3], y: [2, 4]}))
```
`Output`:
```
7.5
[ 3.  7.]
```
Điểm duy nhất khác nhau giữa `tf.placeholder` và `tf.Tensor` đó là placeholder sẽ báo lỗi nếu không có giá trị nào được gán cho nó.

Trong phần tiếp theo, mình sẽ giới thiệu về các thành phần khác của Tensorflow, mọi người chú ý đón xem nhé :D 

**Bài viết được tham khảo và sử dụng hình ảnh từ:**

* [Tensorflow: MNIST For ML Beginners](https://dataplatform.cloud.ibm.com/analytics/notebooks/91440c8b-0bfb-471e-b04e-235e4d9f510d/view?access_token=fb4380415a903111e26cec3bd95d8ba91a04746185c866fecde9d36643fa5585)
* [Low Level APIs from Tensorflow](https://www.tensorflow.org/guide/low_level_intro)