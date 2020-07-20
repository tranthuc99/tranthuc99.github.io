---
layout: post
title: Tự học Tensorflow [P5] - Tạo TFRecord file dùng cho Training
date: 2019-04-25 10:56:20 +0700
description:  tu hoc tensorflow, tao tfrecord
img: 2019-04-25-tu-hoc-tensorflow-p5/tensorflow.jpg # Add image post (optional)
image-dir: /assets/img/2019-06-01-tu-hoc-tensorflow-p5
fig-caption: # Add figcaption (optional)
category: [Tensorflow]
tags: [Self Learning Tensorflow]
permalink: /2019/04/25/tu-hoc-tensorflow-p5
---
Trong phần trước [Tự học Tensorflow [P4] - Linear Regression with Tensorflow]({{site.url}}/2019/03/28/tu-hoc-tensorflow-p4) mình đã thực hiện giải quyết bài toán **`Linear Regression`** bằng những lý thuyết đã giới thiệu trong những bài viết trước đó của **`Tensorflow`**. Hôm nay, mình sẽ tiếp tục giới thiệu cách chuẩn bị dữ liệu training cho bài toán **Classification** đơn giản. Nếu chưa theo dõi nhưng phần trước thì bạn có thể tham khảo thêm:
* [2.Tự học Tensorflow [P2] - Session, Feeding]({{site.url}}/2019/02/26/tu-hoc-tensorflow-p2)
* [3.Tự học Tensorflow [P3] - Dataset, Layer]({{site.url}}/2019/03/11/tu-hoc-tensorflow-p3)
* [4.Tự học Tensorflow [P4] - Linear Regression with Tensorflow]({{site.url}}/2019/03/28/tu-hoc-tensorflow-p4)

* TOC
{:toc}

## Tại sao là TFRecord?
Với dữ liệu trainng dưới dạng hình ảnh, vấn đề thường xuyên gặp phải khi lưu trữ dữ liệu là số lượng file quá lớn, quản lí nhãn cho mỗi ảnh. Để xử lí việc này, giải pháp được đưa ra đó là lưu trữ nhiều hình ảnh dưới 1 hoặc nhiều file để sử dụng trong khi training. `Tensorflow` có cung cấp một định dạng file đó là **`TFRecord`**
## Xử lí dữ liệu
Trước khi được lưu trữ vào file TFRecord, dữ liệu cần được xử lí lại thành định dạng phù hợp cho việc lưu trữ
### Encode file ảnh
Ở đây mình sử dụng package [Pillow](https://pillow.readthedocs.io/) để xử lí hình ảnh. Ảnh sẽ được encode lại bằng `String.IO`
```python
import StringIO
from PIL import Image

image_path="images/kaopiz.png"
io = StringIO.StringIO()
image_pil = Image.open(image_path)
image_width, image_height = image_pil.size
image_pil.save(io, image_format, subsampling=0, quality=100)
image_encoded = io.getvalue()
```
Ở đây ảnh sẽ được encode lại dưới dạng 1 chuỗi string chứ không còn là một ma trận các pixel nữa, điều này giúp cho việc lưu trữ trở nên dễ dàng
### tf.Feature
Với file TFRecord, mỗi bản ghi sẽ được lưu trữ là một **tf.Example**, mỗi `tf.Example` sẽ chứa các **tf.Feature** mang thông tin nhất định của bản ghi đó như hình ảnh, chiều cao, chiều rộng, nhãn, ... . `Tensorflow` cung cấp cho ta các kiểu Feature để lưu trữ trong TFRecord như `tf.train.BytesList`, `tf.train.Int64List`, ... Tùy vào dạng dữ liệu cần lưu trữ mà ta chọn loại Feature phù hợp.
```python
def _int64_feature(value):
    return tf.train.Feature(int64_list=tf.train.Int64List(value=value))
 
def _bytes_feature(value):
    return tf.train.Feature(bytes_list=tf.train.BytesList(value=value))
```


## Tạo file TFRecord
Sau khi dữ liệu đã được xử lí thành định dạng phù hợp, ta tiến hành ghi file.
### tf.train.Example
Tạo một bản ghi để lưu vào file tfrecord:
```python
example = tf.train.Example(features=tf.train.Features(feature={
        'image/encoded': _bytes_feature([image_encoded]),
        'image/class': _bytes_feature(["kaopiz"]),
        'image/height': _int64_feature([image_height]),
        'image/width': _int64_feature([image_width]),
    }))
```
### tf.python_io.TFRecordWriter
Cũng giống như ghi các dạng file khác, chúng ta luôn cần 1 object `writer` để có thể ghi file, bản ghi dưới dạng tf.Example sẽ được serialize để lưu trữ:
```python
output_file_path="data/tf_record/data.tfrecord"
writer = tf.python_io.TFRecordWriter(output_file_path)
writer.write(example.SerializeToString())
writer.close()

```
Có gì cần chia sẻ mọi người để lại bình luận nhé :D 