---
layout: post
title: Quản lí dự án với Github, tại sao không?
date: 2019-12-31 00:16:20 +0700
description: quan li du an voi github, quản lí dự án, github, quản lí dự án bằng github, quản lí task
img: 2019-12-31-quan-li-du-an-voi-github/github.jpg # Add image post (optional)
image-dir: /assets/img/2019-12-31-quan-li-du-an-voi-github
fig-caption: # Add figcaption (optional)
category: [Ultilities]
tags: [Github]
permalink: /2019/12/31/quan-li-du-an-voi-github
---

> Nếu bạn đã và đang sử dụng những tool, platform quản lí task, source code cho những dự án lớn như Jira, Bitbucket, ... thì những tool này thật sự quản lí rất chi tiết và đạt chuẩn. Tuy nhiên nếu bạn thấy việc sử dụng chúng thật rườm rà, đôi khi vì dự án nhỏ, cụ thể là những dự án cá nhân dẫn đến việc sử dụng những tool này không hợp lí gây bất tiện hoặc không thể sử dụng, thì bài viết này là dành cho bạn

* TOC
{:toc}

Hầu hết những công ty Công nghệ phần mềm sẽ xây dựng nhưng quy trình làm việc cho dự án riêng, hoặc sẽ follow một mô hình chuẩn nào đó và tùy biến một chút để vừa với quy mô công ty. Tuy nhiên với những dự án cá nhân, như viết blog hoặc làm việc cùng một đội, nhóm nào đó, thì việc quản lí những task hằng ngày không thể áp dụng một mô hình giống như vậy.

## Github 

Sau một thời gian sử dụng **Trello** để quản lí những task của nhóm, và **Github** để quản lí source code, mình phát hiện ra một vấn đề đó là không thể liên kết những issue của Github với Trello trực tiếp, việc sau khi hoàn thành một issue phải update task Trello gây khá nhiều phiền toái. Thậm chí có những task đã resolve trên Github rồi nhưng trên Trello chưa cập nhật. Sau khi biết đến những feature khác của Github, mình đã quyết định chuyển sang dùng  Github để quản lí các task của dự án. Cụ thể là custom lại mô hình **Scrum**.

## Quản lí dự án cá nhân

Mình sẽ bắt đầu bằng việc khởi tạo một **repository** trên Github, sau đó sẽ là các bước để quản trị các project nhỏ trên repo đó.

### Tạo một repository

Đầu tiên, tạo một repo cá nhân với tên **personal-blog**.

<p align="center"><img alt="Create Repository" src="{{page.image-dir}}/create-repo-1.png"/></p>

Như vậy là mình đã có một repo để có thể lưu trữ code và quản lí task

<p align="center"><img alt="Create Repository" src="{{page.image-dir}}/create-repo-2.png"/></p>

### Tạo một project

Ý tưởng ban đầu là tạo một trang blog cá nhân, nên mình sẽ đặt tên cho dự án này là **Personal Blog**. Tiến hành tạo một Project bằng cách chọn tab `Project` -> `Create a project`:

<p align="center"><img alt="Create Project" src="{{page.image-dir}}/create-project-1.png"/></p>

Sau đó tiến hành thêm những `Column` như **To do** , **In Progress**, **Done** cơ bản:

<p align="center"><img alt="Create Project" src="{{page.image-dir}}/create-project-2.png"/></p>

Với những ai đã quen thuộc với Trello thì layout thật sự rất giống cách quản lí với Trello.

### Tạo Sprint - Milestone

Github cung cấp **Milestone** để quản lí một phase với các mốc thời gian khác nhau. Vì dự định xây dựng blog của mình có rất nhiều chức năng, nên mình sẽ tiến hành chia thành nhiều **Phase** nhỏ, mỗi phase sẽ cập nhật những chắc năng nhất định. **Phase 1** sẽ bao gồm những công việc như sau:

* 1. Tạo Landing page cho blog
* 2. Chức năng đăng nhập, tạo bài viết
* 3. Chức năng comment bài viết

Mình sẽ chia nhỏ các task này thành những Milestone khác nhau, trong **Milestone 1** sẽ gồm **Task 1** và **2**.
Tạo Milestone như sau -> `Issues` -> `Milestones` -> `Create Milestones`
Mình sẽ thực hiện 2 task này trong vòng 1 tuần nên sẽ đặt Milestone này là **Week 1** và chọn ngày deadline là 1 tuần sau đó.

<p align="center"><img alt="Create Milestone" src="{{page.image-dir}}/create-milestone.png"/></p>

### Tạo Task - Issue

Tiếp theo tiến hành tạo những task nhỏ -> `Issues` -> `New Issue`. Trong phần tạo issue có thể chọn những thành phần như:
* **Assignees** - Vì dự án cá nhân nên sẽ giao task cho chính mình
* **Labels** - Có thể lựa chọn những label có sẵn của Github hoặc tự tạo label cho mình, ở đây mình sẽ chọn label cho 2 task là `good fist issue`, bạn cũng có thể tạo những label như `todo` , `in progress`, ...
* **Projects** - Mình sẽ chọn project là `Personal Blog` như đã tạo ở trên
* **Milestone** - Vì 2 task này thuộc Milestone `Week 1` nên mình sẽ chọn `Week 1`

Sau khi tạo mình được 2 task như sau:

<p align="center"><img alt="Create Issue" src="{{page.image-dir}}/create-issue.png"/></p>

### Quản lí các task

Lúc này khi vào phần quản lí Project, sẽ thấy 2 task vừa được tạo nằm trong Project này, việc còn lại sẽ là kéo những task này vào những vị trí tương ứng như khi làm việc với Trello

<p align="center"><img alt="Manage Project" src="{{page.image-dir}}/manage-project.png"/></p>

Việc còn lại là khi hoàn thành các task thì sẽ commit code và đồng thời close issue. Trong khi làm việc nếu có task nào có vấn đề có thể chuyển label hành `question` hoặc comment trực tiếp trong issue đó.

<p align="center"><img alt="Comment on Task" src="{{page.image-dir}}/comment-task.png"/></p>

Với những dự án cá nhân nhỏ, thì việc sử dụng Github để quản lí là hoàn toàn có thể và tiện lợi. Và hơn hết, nó miễn phí với người dùng. Mọi ý kiến đóng góp về bài viết vui lòng để lại ở phần comment. Chúc quý độc giả có một năm mới tràn đầy sức khỏe!
