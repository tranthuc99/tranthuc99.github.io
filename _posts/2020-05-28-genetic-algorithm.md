---
layout: post
title: Genetic Algorithm - Giải thuật di truyền
date: 2020-05-28 00:16:20 +0700
description: genetic algorithm la gi, giai thuat di truyen la gi, giải thuật di truyền là gì, giải thuật di truyền
img: 2020-05-28-genetic-algorithm/evolution.jpg # Add image post (optional)
image-dir: /assets/img/2020-05-28-genetic-algorithm
fig-caption: # Add figcaption (optional)
category: [Architectures and Algorithms]
tags: [Algorithm]
permalink: /2020/05/28/genetic-algorithm
---
Từ lâu, chúng ta thường được nghe rằng loài người được tiến hóa từ loài vượn cổ. Quan điểm này xuất phát từ thuyết tiến hóa của Charles Darwin. Song song với thuyết tiến hóa của Darwin - quan điểm chọn lọc tự nhiên thì còn 1 thuyết tiến hóa nữa của Jean-Baptiste Lamarck. Tuy nhiên trong bài viết này mình sẽ không đề cập tới lĩnh vực này mà mình muốn giới thiệu tới bạn đọc một thuật toán sử dụng chọn lọc tự nhiên để giải quyết bài toán có không gian tìm kiếm rất lớn - **Genetic Algorithm** - *Giải thuật Di Truyền*.

* TOC
{:toc}

### Ý tưởng chính

Nhằm giải thích sự xuất hiện của Hươu cao cổ, Darwin đưa ra giả thiết rằng: "Trong quần thể Hươu vốn đã tồn tại những con Hươu có cổ cao hơn bình thường nhờ gen di truyền và sự đột biến. Trải qua quá trình sinh sống và phát triển, môi trường thay đổi khiến cho thức ăn càng ngày càng khó kiếm hơn, khiến những con Hươu có chiếc cổ cao sẽ chiếm ưu thế sinh tồn hơn. Lâu dần thì thế hệ Hươu mới sẽ được thay bằng những con Hươu cao cổ có khả năng sinh sản và thích nghi với môi trường lớn hơn".

<p align="center"><img alt="Dawwin Evolution Theory" src="{{page.image-dir}}/darwin_evolution_theory.jpg"/></p>

Nhìn vào thuyết tiến hóa của Hươu cao cổ này, chúng ta thấy được sự xuất hiện của những thành phần sau:

* **Quần thể**
* **Đột biến**
* **Sinh sản**
* **Chọn lọc tự nhiên**

đây cũng chính là những thành phần trong giải thuật này.

#### Mối liên hệ giữa các thành phần

Để nắm rõ ý tưởng chính của thuật toán, biểu đồ dưới đây sẽ mổ tả cụ thể những thành phần và mối liên hệ của chúng.

<p align="center"><img alt="Element Relationship" src="{{page.image-dir}}/relationship.png"/></p>

* **1. Population - Quần thể** : Một quần thể ban đầu sẽ có những cá thể nhất định với những đặc tính khác nhau, những đặc tính này sẽ quy định khả năng sinh sản, sinh tồn, khả năng đáp ứng điều kiện môi trường của từng cá thể.
* **2. Natural Selection - Chọn lọc tự nhiên** : Theo thời gian những cá thể yếu hơn, không có khả năng sinh tồn sẽ bị loại bỏ bởi những tác nhân như tranh chấp chuỗi thức ăn, môi trường tác độc, bị loài khác tiêu diệt, ... Cuối cùng sẽ còn lại những cá thể có đặc tính ưu việt hơn sẽ được giữ lại - **Adaptive individual**.
* **3. Mutation - Đột biến** : Như chúng ta đã biết thì mỗi cá thể con được sinh ra sẽ được kế thừa lại những đặc tính của cả cha và mẹ. Sau một thời gian sinh sống, một quần thể sẽ đặt tới giới hạn của các cặp gen của con được tạo nên từ gen của bố mẹ. Để đạt được tới sự tiến hóa, **Đột Biến** chính là một trong những nguyên nhân chính, có vai trò đóng góp nguyên liệu cho quá trình **Chọn lọc tự nhiên**.
* **4. Evolution - Tiến hóa** : Những cá thể đột biến không phải luôn là những cá thể mạnh mẽ và có đủ khả năng sinh tồn, **Chọn lọc tự nhiên** sẽ chọn ra những cá thể đột biến nhưng có thể thích nghi với môi trường sống tốt hơn những cá thể khác trong quần thể. Sau một thời gian sinh sản, những gen đột biến sẽ chiếm ưu thế và chiếm đa số trong quần thể.

<p align="center"><img alt="Butter Fly Population" src="{{page.image-dir}}/butter_fly_population.jpg"/></p>

### Thuật toán di truyền

Có rất nhiều cách giải thích và biểu đồ khác nhau để diễn giải thuật toán, nhưng nhìn chung các thành phần chính của thuật toán sẽ không thay đổi.

<p align="center"><img alt="Algorithm Graph" src="{{page.image-dir}}/graph.png"/></p>

Xét bài toán **Tìm mật khẩu**, yêu cầu của bài toán như sau:
* Mật khẩu gồm 8 kí tự ( bao gồm chữ cái, chữ số và khoảng trắng) - Ví dụ: **hoilamgi**.
* Mỗi lần thử, hệ thống sẽ báo về số lượng kí tự đúng với mật khẩu.
* Yêu cầu tìm ra chuỗi mật khẩu cho trước.

Thuật toán sẽ dừng lại khi tìm được cá thể đáp ứng được nhu cầu đề ra sau mỗi thế hệ mới. Quá trình sản sinh thế hệ tiếp theo sẽ là một vòng lặp (**Evaluation Fitness** -> **Selection** -> **Crossover** -> **Mutation**). Chúng ta sẽ cùng xây dựng những thành phần chính trong thuật toán để giải quyết bài toán này.

#### Initial Population- Khởi tạo quần thể

Nhìn chung thuật toán sẽ mô phỏng lại hầu hết những hiện tượng xảy ra trong quá trình tiến hóa của động vật. Vì vậy để thuật toán có thể vận hành được, thì điều đầu tiên cần có chính là **Quần thể**. Xét bài toán tìm mật khẩu, quần thể sẽ bao gồm những chuỗi 8 kí tự, được sinh ra ngẫu nhiên.

<p align="center"><img alt="Initial Population" src="{{page.image-dir}}/step1.png"/></p>

#### Evaluation Fitness - Đánh giá năng lực

Tiếp theo, mỗi chuỗi mật khẩu sẽ được đánh giá sự chính xác so với mật khẩu cho trước, với mỗi kí tự giống với mật khẩu cho trước tại đúng vị trí sẽ được **1 point**.

<p align="center"><img alt="Evaluation Fitness" src="{{page.image-dir}}/step2.png"/></p>

Thành phần **Point** ở đây sẽ đại diện cho khả năng sinh tồn của cá thể trong quần thể, càng lớn tức cá thể đó càng thích nghi với môi trường tốt.

#### Selection - Chọn lọc

Sau khi đã đánh giá được quần thể, các cá thể có khả năng sinh tồn tốt hơn sẽ có cơ hội được sinh sản nhiều hơn các cá thể còn lại. Các chuỗi kí tự mật khẩu sẽ được lựa chọn theo số **Point** đang có.

<p align="center"><img alt="Selection" src="{{page.image-dir}}/step3.png"/></p>

#### Crossover - Sinh sản

Vào giai đoạn sinh sản, các cá thể con sẽ được kế thừa các đặc tính từ cả bố và mẹ. Thông thường, cá thể con sẽ nhận một nửa gen từ mỗi bố, mẹ.

<p align="center"><img alt="Crossover 1 Point" src="{{page.image-dir}}/step4.png"/></p>

Cá thể con có thể sẽ thích nghi tốt hơn, hoặc kém hơn. Ngoài ra, có những kiểu lai tạo khác nhau như **2 Point**, **Uniform Selection**.

<p align="center"><img alt="Crossover 2 Point" src="{{page.image-dir}}/step5.png"/></p>
<p align="center">*2 Point*</p>

<p align="center"><img alt="Crossover Uniform" src="{{page.image-dir}}/step6.png"/></p>
<p align="center">*Uniform*</p>

#### Mutation - Đột biến

Dễ nhận thấy rằng, nếu chỉ bằng việc sinh ngẫu nhiên và lai tạo, sẽ rất khó để tìm được nghiệm. Trừ khi cá thể khởi tạo phù hợp luôn với yêu cầu đề bài, tức là có đáp án luôn từ đầu - Ăn May. Như đã nói từ đầu, **Đột biến** chính là nguyên liệu của **Chọn lọc tự nhiên**, bằng việc lựa chọn ngẫu nhiên các vị trí và thay thế bằng một kí tự ngẫu nhiên nào đó, chúng ta có thể mô phỏng lại hiện tượng đột biến - [Đột biến điểm](https://vi.wikipedia.org/wiki/%C4%90%E1%BB%99t_bi%E1%BA%BFn_%C4%91i%E1%BB%83m)

<p align="center"><img alt="Mutation" src="{{page.image-dir}}/step7.png"/></p>

Các cá thể đột biến có thể sẽ có khả năng thích nghi tốt hơn (1 -> 2), hoặc cũng có thể ngược lại (4 -> 3). Quá trình này sẽ lặp lại cho đến khi tìm được đáp án phù hợp.

### Những vấn đề thường gặp khi sử dụng thuật toán

Thông thường, **Thuật toán Di Truyền** sẽ đạt được lợi thế trong những bài toán có không gian tìm kiếm quá lớn mà những giải thuật vét cạn không thể xử lí được. Tuy nhiên khi sử dụng, chúng ta sẽ cần cân nhắc những vấn đề ảnh hưởng tới việc lựa chọn thuật toán.

#### Evaluation Fitness

Như trong bộ truyện One Punch Man, các anh hùng sẽ được đánh giá và xếp hạnh theo **Rank**. **Rank** sẽ nói lên dược năng lực và sức mạnh của người đó - ngoại trừ **Thánh Saitama**. Vấn đề đầu tiên gặp phải đó chính là việc đánh giá các cá thể trong quần thể, chúng ta cần một phương thức để có thể đánh giá sự thích nghi hay ưu thế của từng cá thể.

<p align="center"><img alt="Evaluation Fitness" src="{{page.image-dir}}/evaluation_fitness.png"/></p>

Tuy nhiên khi thực hiện việc này, chúng ta sẽ cần cân nhắc 2 vấn đề:
* **Tính khả thi**: Việc tìm phương thức đánh giá không phải luôn khả thi, ví dụ trong bài toàn tìm một giai điệu mới, việc đánh giá giai điệu đó có "dễ nghe", "hay" hay không thuộc về cảm quan của mỗi người, nên việc tìm một hàm đánh giá chính xác sẽ rất khó khăn.
* **Chi phí**: Đây cũng là một vấn đề cần cân nhắc, nếu chi phí tính toán của phương thức đánh giá quá lớn, việc sử dụng để tìm kiếm trong không gian sẽ mất nhiều thời gian, thậm chí lâu hơn vét cạn.

#### Chromosome

Trong bài toán **Tìm mật khẩu** thì chuỗi kí tự đại diện cho các **Nhiễm sắc thể** hay **Chuỗi ADN**. Việc số hóa những đặc tính của bài toán thành các bits, bytes để có thể sử dụng trong khâu **Crossover**, **Mutation** cũng gặp nhiều khó khăn trong những bài toán khác nhau. Bởi với mỗi sự thay đổi bit sẽ phải tương ứng với việc tạo ra một cá thể với đặc tính khác nhau.

<p align="center"><img alt="Anten của Nasa dùng thuật toán di truyền để tìm hình dáng tối ưu thu phát sóng." src="{{page.image-dir}}/anten_nasa.png"/></p>

<p align="center">*Anten của Nasa dùng thuật toán di truyền để tìm hình dáng tối ưu thu phát sóng*</p>

### Ứng dụng thực tiễn

Như đã đề cập trước đó, **Giải thuật Di truyền** hay **Genetic Algorithm** sẽ có ưu thế trong những bài toán mà có sẵn lời giải trong một không gian tìm kiếm lớn như những bài toán cổ điển *Người du lịch*, *Cái Túi*, ... Ngoài ra, hiện nay với việc bùng nổ của **AI**, việc sử dụng mạng **Neural** cùng với giải thuật này cũng giúp giải quyết một số bài tóan về tự động, hành vi, ...

<p align="center"><img alt="Snake Fusion" src="{{page.image-dir}}/snake_fusion.gif"/></p>

Mình xin kết thúc bài viết ở đây, mong nhận được phản hồi của bạn đọc!

Bài viết được tham khảo và sử dụng hình ảnh từ:
* [Neural Network Learns to Play Snake](https://www.youtube.com/watch?v=zIkBYwdkuTk)
* [Genetic Algorithm made intuitive with natural selection and python
](https://medium.com/datadriveninvestor/genetic-algorithm-made-intuitive-with-natural-selection-and-python-project-from-scratch-3462f7793a3f)
* [Giraffe Evolution](https://id.pinterest.com/pin/310959549252410148/)