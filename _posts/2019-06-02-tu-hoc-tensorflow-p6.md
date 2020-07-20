---
layout: post
title: Tự học Tensorflow [P6] - Tensors
date: 2019-05-28 10:56:20 +0700
description: tu hoc tensorflow, tensor la gi
img: 2019-06-02-tu-hoc-tensorflow-p6/tensorflow.jpg # Add image post (optional)
image-dir: /assets/img/2019-06-01-tu-hoc-tensorflow-p6
fig-caption: # Add figcaption (optional)
category: [Tensorflow]
tags: [Self Learning Tensorflow]
permalink: /2019/05/28/tu-hoc-tensorflow-p6
---
Tiếp tục series về Tensorflow, trong phạm vi bài viết này, mình sẽ đi sâu vào một thành phần chính của Tensorflow đó chính là **tf.Tensor**. Nếu đây là lần đầu tiếp xúc với Tensorlfow, bạn có thể tham khảo những bài viết trước của mình.
* [1.Tự học Tensorflow [P4] - Linear Regression with Tensorflow]({{site.url}}/2019/03/28/tu-hoc-tensorflow-p4)
* [2.Tự học Tensorflow [P5] - Tạo TFRecord file dùng cho Training]({{site.url}}/2019/04/25/tu-hoc-tensorflow-p5)

* TOC
{:toc}

## Tensor là gì 
Như mình đã đề cập ở [phần 1]({{site.url}}/tu-hoc-tensorflow-p1), đơn vị dữ liệu chính của Tensorflow là Tensor. Tensor là thể hiện của các vector, matrix với số chiều lớn. Tensorflow biểu diễn Tensor như một mảng của các kiểu dữ liệu cơ bản với số chiều lớn. 
Khi viết một chương trình Tensorflow thì đối tượng mà chúng ta thao tác và xử lí nhiều nhất là **`tf.Tensor`**. Một đối tượng `tf.Tensor` sẽ định nghĩa một phần tính toán và cuối cùng trả về giá trị của nó khi được khởi chạy. Một chương trình Tensorflow là một Graph có chứa các Tensor được liên kết với nhau, định nghĩa các mà mỗi Tensor được tính toán mà mối quan hệ giữa các Tensor.
Một Tensor bao gồm 2 thành phần chính là :

* `data type` - kiểu dữ liệu: có thể là float32, int32, string ...
* `shape` - hình dáng: mang thông tin về số chiều, kích thước mỗi chiều của Tensor.

Mỗi phần tử của Tensor đều có chung một kiểu dữ liệu, và kiểu dữ liệu của mỗi `Tensor` luôn được biết trước. `Shape` của một `Tensor` không phải luôn luôn được biết trước. Hầu hết các `Operation` sẽ trả về các `Tensor` mang thông tin shape nếu đầu vào của nó là các `Tensor` đã định nghĩa shape. Trong một vài trường hợp nhất định, thì chỉ có thể biết được shape của `Tensor` khi khởi chạy graph.
Một số loại Tensor có thể kể đến:
* `tf.Variable`
* `tf.constant`
* `tf.placeholder`
* `tf.SparseTensor`

Riêng với **tf.Variable**, thì giá trị của nó là không thể thay đổi trong một lần thực thi.
### Tensor Rank
**Rank** của một Tensor là số lượng chiều của nó. Khái niệm rank ở đây không giống trong toán học. 

| Rank | Math entity |
|--------|------------------|
|0|Scalar|
|1|Vector|
|2|Matrix|
|3|3-Tensor|
|n|n-Tensor|

#### Rank 0
Tensor rank 0 là Tensor chỉ chứa một giá trị đơn lẻ của đối tượng.
```python
mammal = tf.Variable("Elephant", tf.string)
ignition = tf.Variable(451, tf.int16)
floating = tf.Variable(3.14159265359, tf.float64)
its_complicated = tf.Variable(12.3 - 4.85j, tf.complex64)
```
Trong Tensorflow, String được coi là một đối tượng riêng lẻ, không phải là một chuỗi các kí tự, nên có thể tồn tại các vector String.
#### Rank 1
Tensor rank 1 là một list các giá trị.
```python
mystr = tf.Variable(["Hello"], tf.string)
cool_numbers  = tf.Variable([3.14159, 2.71828], tf.float32)
first_primes = tf.Variable([2, 3, 5, 7, 11], tf.int32)
its_very_complicated = tf.Variable([12.3 - 4.85j, 7.5 - 6.23j], tf.complex64)
```
#### Rank cao hơn
Tensor rank cao hơn có thể tạo bằng cách định nghĩa các ma trận bên trong
```python
mymat = tf.Variable([[7],[11]], tf.int16)
myxor = tf.Variable([[False, True],[True, False]], tf.bool)
linear_squares = tf.Variable([[4], [9], [16], [25]], tf.int32)
squarish_squares = tf.Variable([ [4, 9], [16, 25] ], tf.int32)
rank_of_squares = tf.rank(squarish_squares)
mymatC = tf.Variable([[7],[11]], tf.int32)
```
Ngoài ra, ta có thể tạo một Tensor với shape mong muốn
```python
my_image = tf.zeros([10, 299, 299, 3])  # batch x height x width x color
```
#### Lấy giá trị rank của Tensor
Để trả về giá trị rank của một Tensor, Tensorflow cung cấp phương thức **tf.rank**:
```python
r = tf.rank(my_image)
#Khi khởi chạy Graph, r sẽ có giá trị là 4.
```
#### Truy cập vào các chiều của Tensor
Để truy cập được vào các thành phần phía trong Tensor, ta cần định nghĩa chỉ mục cần truy cập
* Với rank 0 thì không cần vì Tensor luôn trả về một giá trị duy nhất
* Rank 1 - vector: dùng 1 số để truy cập

```python
my_scalar = my_vector[2]
```
* rank 2 - matrix: dùng 2 số để truy cập

```python
my_scalar = my_matrix[1, 2]
```
Tương tự như cách sử dụng Numpy, ta có thể lấy được các vector, matrix nhỏ từ tập matrix lớn hơn:
```python
my_row_vector = my_matrix[2]
my_column_vector = my_matrix[:, 3]
```
### Tensor Shape
Shape của Tensor là số phần tử ở mỗi chiều của Tensor. Tensorflow tự động tính toán shape trong quá trình training.

|Rank|Shape|Example|
|-------|---------|-------------|
|0|[]|A Scalar|
|1|[D0]|A 1-D tensor with shape[5]|
|2|[D0, D1]|A 2-D tensor with shape[3,4]|
|3|[D0, D1, D2]|A 3-D tensor with shape[1,4,3]|
|n|[D0, D1, ..., D<sub>n-1</sub>]|A tensor with shape [D0, D1, ..., D<sub>n-1</sub>]|

Shape có thể được biểu diễn bởi Python list, tuple kiểu integer hoặc với **tf.TensorShape**
#### Lấy giá trị shape của Tensor
Có 2 cách để có thể lấy được shape của Tensor. 
* Đọc thuộc tính `shape` của Tensor. Nó sẽ trả về một đối tượng `TensorShape`.
* Sử dụng phương thức `tf.shape`.

```python
shape = my_matrix.shape
or 
shape = tf.shape(my_matrix)
```
#### Thay đổi shape của Tensor
Số lượng các phần tử trong một Tensor có thể được tính từ giá trị shape của nó, và dĩ nhiên nhiều Tensor có shape khác nhau nhưng số lượng phần tử có thể bằng nhau. Nếu bạn đã quen thuộc với việc dùng `numpy.reshape` thì Tensorfloư cũng cung cấp 1 phương thức như vậy.
```python
rank_three_tensor = tf.ones([3, 4, 5])
matrix = tf.reshape(rank_three_tensor, [6, 10])  # Reshape về ma trận 6x10
             
matrixB = tf.reshape(matrix, [3, -1])  #  Reshape về ma trận 3x20
	                                   # -1 ở đây mang hàm ý là tự tính size của shape
matrixAlt = tf.reshape(matrixB, [4, 3, -1]) 

yet_another = tf.reshape(matrixAlt, [13, 2, -1])  # ERROR!
```
Bạn có thể reshape về bất cứ shape nào mong muốn miễn là bảo toàn số lượng phần tử phía trong Tensor là như nhau.
### Data types
Không thể tồn tại một Tensor mà có nhiều hơn 1 kiểu dữ liệu, nếu muốn thì bạn chỉ có thể chuyển nó về dạng cấu trúc String. Ngoài ra, chúng ta có thể chuyển datatype của Tensor từ loại này sang loại khác bằng phương thức **tf.cast**:
```python
float_tensor = tf.cast(tf.constant([1, 2, 3]), dtype=tf.float32)
```

Để xem kiểu data là gì, ta có thể sử dụng trực tiếp thuộc tính **`Tensor.dtype`**
Khi khởi tạo một Tensor, nếu không định nghĩa kiểu dữ liệu cho Tensor, Tensorflow sẽ tự định nghĩa kiểu dữ liệu mà có thể thể hiện dữ liệu bên trong Tensor đó.

### Xem giá trị của một Tensor
Cách đơn giản nhất để kiểm tra giá trị của một Tensor là dùng phương thức **`Tensor.eval`**:
```python
constant = tf.constant([1, 2, 3])
tensor = constant * constant
print(tensor.eval())
```
Lưu ý là chỉ có thể sử dụng eval khi đã kích hoạt một **tf.Session** default.
Một cách nữa có thể kể đến là dùng **tf.Print**. Giá trị của Tensor sẽ tự động được in khi Tensor tf.Print được eval bởi Session.
```python
t = <<some tensorflow operation>>
t = tf.Print(t, [t])  # Here we are using the value returned by tf.Print
result = t + 1
```
Khi eval Tensor `result`, giá trị của `t` sẽ tự động được in.
Như vậy, chúng ta vừa tìm hiểu sâu thêm về Tensor. Để lại comment nếu có gì cần chia sẻ nhé :D 

**Bài viết được tham khảo và sử dụng hình ảnh từ:**
* [1.Tensors - Tensorflow](https://www.tensorflow.org/guide/tensors#getting_a_tftensor_objects_rank)