---
layout: post
title: Tự học Tensorflow [P4] - Linear Regression with Tensorflow
date: 2019-03-28 10:56:20 +0700
description: tu hoc tensorflow, linear regression
img: 2019-03-28-tu-hoc-tensorflow-p4/tensorflow.jpg # Add image post (optional)
image-dir: /assets/img/2019-03-28-tu-hoc-tensorflow-p4
fig-caption: # Add figcaption (optional)
category: [Tensorflow]
tags: [Self Learning Tensorflow]
permalink: /2019/03/28/tu-hoc-tensorflow-p4
---
Ngoài những bài học lí thuyết từ phần trước [Tự học Tensorflow [P3] - Dataset, Layer]({{site.url}}/2019/03/11/tu-hoc-tensorflow-p3) thì chúng ta cũng cần những giờ thực hành để ghi nhớ kiến thức, trong phạm vi bài viết này mình sẽ chia sẻ về cách giải quyết một bài toán **`Linear Regression`** đơn giản bằng **Tensorflow**. Nếu chưa theo dõi nhưng phần trước thì bạn có thể tham khảo thêm:
* [1.Tự học Tensorflow [P1] - Tensor, Graph, TensorBoard]({{site.url}}/tu-hoc-tensorflow-p1)
* [2.Tự học Tensorflow [P2] - Session, Feeding]({{site.url}}/2019/02/26/tu-hoc-tensorflow-p2)
* [3.Tự học Tensorflow [P3] - Dataset, Layer]({{site.url}}/2019/03/11/tu-hoc-tensorflow-p3)

* TOC
{:toc}

## Linear Regression là gì?
[**Linear Regression**](https://en.wikipedia.org/wiki/Linear_regression) hay còn được gọi là **Hồi Quy Tuyến Tính** là một trong những thuật toán *Supervised learning* cơ bản của Machine Learning. Chắc hẳn khi học THCS hay THPT chúng ta đã khá quen thuộc với phương trình hàm số `y = ax + b` với `x` là biến số. Tuy nhiên ngược lại với bài toán này, ta coi `a` và `b` là biến số và tiến hành tìm `a` và `b` dựa trên tập `x` và `y` có sẵn. Bài toán này trong thực tế có khá nhiều, ví dự như khi ta muốn dự đoán `số năm kinh nghiệm` của một PHP Developer dựa trên số tuổi của lập trình viên đó trên tập dữ liệu của toàn bộ lập trình viên đã làm việc trước đó tại công ty.

|ID        | Age           | Experience Years  |
|---------:|--------------:|---------------------------|
|0|22|1|
|1|25|3|
|2|27|2|
|3|25|4|

Vậy công việc là với mỗi đầu vào là số tuổi của `developer` thì sẽ trả lại số năm kinh nghiệm dự đoán `developer` đó đã làm. Dĩ nhiên với loại bài toán này thì dữ liệu cũng tồn tại nhiễu ví dụ như `developer - 2` với `27` tuổi nhưng số năm kinh nghiệm lại chỉ có `2`.

## Cách giải quyết bài toán
Công việc chúng ta cần làm ở đây là tìm mối liên hệ giữa độ tuổi và số năm kinh nghiệm của một `Developer`, mối quan hệ này được gọi là **`hypothesis`**, trong trường hợp này thì nó chính là một đường thẳng trong không gian 2 chiều.

<h2>$$h(x) = wx+b$$</h2>

Ở đây `w` là một vector `Weights` còn `b` là một scalar `Bias`. Chúng được coi là các `parameters` của một model.
Để có thể tìm được `w` và `b` thì chúng ta cần ước lượng giá trị của chúng dựa vào hàm chi  phí **Loss**.

<h2>$$J(w,b) = \frac{1}{2m}\sum_{i=1}^m (y_i - h(x_i))^2$$</h2>

Hàm chi phí được dùng ở đây là hàm **`Mean Squared Error`** - Trung Bình Bình Phương Độ Lệch được tính bằng trung bình cộng của bình phương độ lệch trên mỗi điểm dữ liệu. Để tối ưu hàm này chúng ta sử dụng phương pháp **`Gradient Descent (GD)`** - một trong những phương pháp nền móng của Deep Learning. Với GD thì công việc của chúng ta là cập nhật lại giá trị của  $$\omega$$ và $$b$$ dựa trên đạo hàm của hàm `Loss`.
<h2>
$$\omega = \omega - \alpha * \frac{\delta J}{\delta w}$$
$$b = b - α * \frac{\delta J}{\delta b}$$
</h2>
$$\alpha$$ ở đây cũng là một `hyperparameter` với tên gọi **`Learning rate`**.

## Triển khai với Tensorflow
Với những bạn chưa biết **Tensorflow** là gì thì nên tham khảo bài viết [Tự học Tensorflow [P1]]({{site.url}}/tu-hoc-tensorflow-p1).
Chúng ta bắt đầu bằng việc khai báo những thư viện cần thiết
{% highlight python%}
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
{% endhighlight %}
Để giữ nguyên giá trị random của `Numpy` và `Tensorflow`, chúng ta cố định `seed`.
{% highlight python %}
np.random.seed(101) 
tf.set_random_seed(101) 
{% endhighlight %}
Tiếp theo chúng ta khởi tạo dữ liệu và thêm nhiễu:
{% highlight python%}
ages = np.linspace(20, 50, 50) # Độ tuổi từ khoảng 20-50
exp_year = np.linspace(1, 20, 50)# Số năm kinh nghiệm 1-20

ages += np.random.uniform(-1, 1, 50)
exp_year += np.random.uniform(-1, 1, 50)
{% endhighlight%}
Data thu được:
<p align="center"><image src="{{page.image-dir}}/data.png"/></p>

Giờ chúng ta bắt đầu tạo model bằng cách sử dụng `tf.placeholder` cho `ages` và `exp_year` để có thể `feed` tập dữ liệu training vào **optimizer** trong suốt quá trình training.
{% highlight python%}
X = tf.placeholder("float") 
Y = tf.placeholder("float") 
{% endhighlight%}

Tiếp theo chúng ta khởi tạo 2 `tf.Variable` để lưu giá trị của **Weights** và **Bias**. Thông thường, chúng ta có thể khởi tạo Weights bằng cách sử dụng lớp `tf.layers.Dense`.
{% highlight python%}
W = tf.Variable(np.random.randn(), name = "W")
b = tf.Variable(np.random.randn(), name = "b")
{% endhighlight%}
Khởi tạo giá trị `learning rate` cho optimizer và số lượng epochs:
{% highlight python%}
learning_rate = 0.02
training_epochs = 1000
{% endhighlight%}
Khởi tạo optimizer, operation tính giá trị loss:
{% highlight python%}
# Dự đoán số năm kinh nghiệm dựa trên Weight và Bias
exp_year_pred = tf.add(tf.multiply(AGE, W), b)

# Tính loss Mean Squared Error
cost = tf.reduce_sum(tf.pow(exp_year_pred - EXP, 2)) / (2 * n)

# Gradient Descent Optimizer
optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)

# Khởi tạo giá trị cho Graph
init = tf.global_variables_initializer()
{% endhighlight%}
Tensorflow có cung cấp hàm loss được xây dựng sẵn đó là `tf.losses.mean_squared_error`. Sau khi đã khởi tạo xong mọi thứ, tiến hành training và log lại thay đổi của `Weights` và `Bias`:
{% highlight python%}
# Chạy Session
with tf.Session() as sess:

    # Khởi chạy hàm khởi tạo
    sess.run(init)
    for epoch in range(training_epochs):

        # Sử dụng feed_dict để truyền data vào Optimizer
        for (age, exp) in zip(ages, exp_year):
            sess.run(optimizer, feed_dict={AGE: age, EXP: exp})

            # Log Loss, Weights và Bias sau mỗi 100 epochs
        if (epoch + 1) % 100 == 0:
            # Tính giá trị loss
            c = sess.run(cost, feed_dict={AGE: ages, EXP: exp_year})
            print("Epoch", (epoch + 1), ": cost =", c, "W =", sess.run(W), "b =", sess.run(b))

    # Tính giá trị loss sau khi đã training xong
    training_cost = sess.run(cost, feed_dict={AGE: ages, EXP: exp_year})
    weight = sess.run(W)
    bias = sess.run(b)

    # Dự đoán số năm kinh nghiệm dưa trên weights và bias vừa train
    age_predictions = weight * ages + bias
    print("Training cost =", training_cost, "Weight =", weight, "bias =", bias, '\n')

{% endhighlight%}
Kết quả sau 1000 epochs:
<p align="center"><image src="{{page.image-dir}}/0.02_1000.png"/></p>

Model có vẻ chưa fit, training với 5000 epochs:
{% highlight python%}
Epoch 100 : cost = 10.946286 W = 0.38977066 b = 0.8715187
Epoch 200 : cost = 10.205628 W = 0.39913383 b = 0.40359172
Epoch 300 : cost = 9.51677 W = 0.40815943 b = -0.047464404
Epoch 400 : cost = 8.876068 W = 0.41685966 b = -0.48225853
Epoch 500 : cost = 8.280136 W = 0.42524618 b = -0.9013753
Epoch 600 : cost = 7.7258153 W = 0.4333303 b = -1.305382
Epoch 700 : cost = 7.2101865 W = 0.44112304 b = -1.6948253
Epoch 800 : cost = 6.7305303 W = 0.44863477 b = -2.0702267
Epoch 900 : cost = 6.284323 W = 0.4558757 b = -2.4320927
Epoch 1000 : cost = 5.869206 W = 0.4628556 b = -2.780915
Epoch 1100 : cost = 5.4830136 W = 0.46958375 b = -3.1171532
Epoch 1200 : cost = 5.123692 W = 0.47606948 b = -3.4412775
Epoch 1300 : cost = 4.7893686 W = 0.4823213 b = -3.7537127
Epoch 1400 : cost = 4.4782953 W = 0.48834747 b = -4.0548725
Epoch 1500 : cost = 4.188843 W = 0.49415618 b = -4.3451643
Epoch 1600 : cost = 3.9194515 W = 0.49975616 b = -4.625025
Epoch 1700 : cost = 3.668752 W = 0.5051542 b = -4.894793
Epoch 1800 : cost = 3.4354403 W = 0.51035756 b = -5.1548295
Epoch 1900 : cost = 3.2182944 W = 0.51537305 b = -5.4054804
Epoch 2000 : cost = 3.0161898 W = 0.5202075 b = -5.6470833
Epoch 2100 : cost = 2.8280525 W = 0.52486783 b = -5.879984
Epoch 2200 : cost = 2.6529238 W = 0.52936 b = -6.10448
Epoch 2300 : cost = 2.4898534 W = 0.5336909 b = -6.3209205
Epoch 2400 : cost = 2.3380504 W = 0.53786534 b = -6.5295362
Epoch 2500 : cost = 2.1967044 W = 0.5418894 b = -6.7306385
Epoch 2600 : cost = 2.06509 W = 0.5457683 b = -6.9244866
Epoch 2700 : cost = 1.9425381 W = 0.5495069 b = -7.1113243
Epoch 2800 : cost = 1.8283958 W = 0.5531111 b = -7.2914443
Epoch 2900 : cost = 1.7221025 W = 0.55658495 b = -7.46505
Epoch 3000 : cost = 1.6230927 W = 0.5599336 b = -7.6323986
Epoch 3100 : cost = 1.5308691 W = 0.563161 b = -7.793693
Epoch 3200 : cost = 1.444925 W = 0.5662735 b = -7.949237
Epoch 3300 : cost = 1.3649043 W = 0.5692716 b = -8.099068
Epoch 3400 : cost = 1.2902647 W = 0.57216465 b = -8.243648
Epoch 3500 : cost = 1.220729 W = 0.57495254 b = -8.382976
Epoch 3600 : cost = 1.1559719 W = 0.5776382 b = -8.51719
Epoch 3700 : cost = 1.0955786 W = 0.5802284 b = -8.646636
Epoch 3800 : cost = 1.0392704 W = 0.58272594 b = -8.77145
Epoch 3900 : cost = 0.9868592 W = 0.58512956 b = -8.891571
Epoch 4000 : cost = 0.9379176 W = 0.58745015 b = -9.007542
Epoch 4100 : cost = 0.8922657 W = 0.5896877 b = -9.119366
Epoch 4200 : cost = 0.8497285 W = 0.59184283 b = -9.227069
Epoch 4300 : cost = 0.81009895 W = 0.59391785 b = -9.33077
Epoch 4400 : cost = 0.77307904 W = 0.595921 b = -9.430876
Epoch 4500 : cost = 0.738523 W = 0.597853 b = -9.527429
Epoch 4600 : cost = 0.7063254 W = 0.5997128 b = -9.620371
Epoch 4700 : cost = 0.6762528 W = 0.6015071 b = -9.710042
Epoch 4800 : cost = 0.6482169 W = 0.60323465 b = -9.796377
Epoch 4900 : cost = 0.62203795 W = 0.60490054 b = -9.87963
Epoch 5000 : cost = 0.5976187 W = 0.6065048 b = -9.9598055
Training cost = 0.5976187 Weight = 0.6065048 bias = -9.9598055 
{% endhighlight%}
<p align="center"><image src="{{page.image-dir}}/0.02_5000.png"/></p>
Training với 10000 epochs:
<p align="center"><image src="{{page.image-dir}}/0.02_10000.png"/></p>

Như vậy mình vừa hướng dẫn các bạn giải một bài toán Linear Regression đơn giản bằng `Tensorflow`. Nếu bài viết có gì chưa đúng vui lòng comment dưới phần bình luận :D. Trong bài viết sau mình sẽ giới thiệu chi tiết về các thành phần khác như Loss, Optimizers.