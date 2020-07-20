---
layout: post
title: Cách sử dụng Miniconda để quản lý Python Packages
date: 2019-01-24 07:56:20 +0700
description:  miniconda, cach su dung miniconda, python packages
img: 2019-01-24-cach-su-dung-miniconda-de-quan-ly-python-packages/python_env_management_with_conda.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
image-dir: /assets/img/2019-01-24-cach-su-dung-miniconda-de-quan-ly-python-packages
category: [Ultilities]
tags: [Miniconda, Python Packages]
permalink: /2019/01/24/cach-su-dung-miniconda-de-quan-ly-python-packages
---
>Khi lập trình với Python, việc phải cài đặt thêm các package là điều dĩ nhiên cần phải có. Điều này có thể dễ dàng đạt được với câu lệnh `pip install <package_name>.` Tuy nhiên, khi làm việc với một dự án lớn, việc thay đổi phiên bản python, hay phiên bản của các package có thể gây khá nhiều phiền toái cho những bạn mới bắt đầu. Trong bài viết này, mình sẽ hướng dẫn cách để có thể quản lý các python package với công cụ Miniconda.

* TOC
{:toc}

## Đôi điều về Miniconda

Có thể một số bạn đã quen với việc sử dụng Anaconda vì nó cung cấp khá đầy đủ những package khi cài đặt. Tuy nhiên, việc thừa thãi các package có sẵn sẽ chiếm dung lượng bộ nhớ của bạn. Thêm vào đó nó cũng gây khá nhiều phiền toái khi thay đổi phiên bản cho các package. Miniconda giải quyết những vấn đề phía trên, giúp bạn có thể dễ dàng quản lý phiên bản thông qua các **Virtual Environment (env)**. Hiểu đơn giản là với mỗi một dự án cần cài đặt các phiên bản python hay package khác nhau, bạn chỉ cần lưu chúng trong một env nhất định, và khi chuyển qua làm việc với dự án khác, việc bạn cần làm đơn giản chỉ là đổi các env.

## Cài đặt

Miniconda có thể hoạt động trên cả 3 hệ điều hành Windows/Mac OS X/Linux. Bạn có thể tải file cài đặt tại [đây](https://conda.io/en/latest/miniconda.html)

<p align="center"><image src="{{page.image-dir}}/miniconda_install.png"/></p>

## Sử dụng

### Kiểm tra phiên bản

Sau khi cài đặt, bạn có thể kiểm tra phiên bản của Miniconda với câu lệnh:

```
$ conda -V
conda 4.5.12
```

### Tạo ENV mới

Khi khởi tạo env, bạn có thể tuỳ chọn phiên bản Python phù hợp với câu lệnh:

`conda create -n <env_name> python=x.x`

VD: Khởi tạo env với tên gọi **kaopiz** và phiên bản **python 2.7**
```
conda create -n kaopiz python=2.7

The following NEW packages will be INSTALLED:
ca-certificates: 2018.12.5-0 
certifi: 2018.11.29-py27_0 
libcxx: 4.0.1-hcfea43d_1 
libcxxabi: 4.0.1-hcfea43d_1 
libedit: 3.1.20181209-hb402a30_0
libffi: 3.2.1-h475c297_4 
ncurses: 6.1-h0a44026_1 
openssl: 1.1.1a-h1de35cc_0 
pip: 18.1-py27_0 
python: 2.7.15-h8f8e585_6 
readline: 7.0-h1de35cc_5 
setuptools: 40.6.3-py27_0 
sqlite: 3.26.0-ha441bb4_0 
tk: 8.6.8-ha441bb4_0 
wheel: 0.32.3-py27_0 
zlib: 1.2.11-h1de35cc_3 
Proceed ([y]/n)?
```

Chọn **Y** để tiến hành khởi tạo env.

### Khởi động ENV

Sau khi tạo một env, bạn cần kích hoạt env đó với cú pháp lệnh sau:

`source activate <env_name>`

Nếu không nhớ tên của env là gì, dùng câu lệnh conda env list để liệt kê tất cả các env mà Miniconda quản lý.
Sau khi kích hoạt, conda sẽ tạo một session mới với PATH và danh sách package tương ứng với env được kích hoạt. Bạn có thể dùng câu lệnh `pip list` hay `python --version` để kiểm tra.
Lúc này bạn có thể install những package cần thiết bằng `pip`

### Dừng ENV
Khi không còn muốn dùng env nữa, sử dụng câu lệnh:
`source deactivate` để dừng session hiện hành lại, hoặc cũng có thể chuyển qua env khác với câu lệnh 
`source activate <env_name>`

### Tổng kết
Hy vọng qua bài viết này, bạn có thể nắm cơ bản cách sử dụng miniconda để quản lý Python. Với những bạn mới bắt đầu thì Anaconda là sự lựa chọn phù hợp để làm quen nhưng khi làm việc với nhiều dự án khác nhau, thì việc sử dụng Miniconda để quản lý là thực sự cần thiết.

**Bài viết được tham khảo và sử dụng hình ảnh từ:**
<br>
[1. Python Environment Management with Conda](https://school.geekwall.in/p/S1_1yfaPz/python-environment-management-with-conda)