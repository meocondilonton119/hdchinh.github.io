---
layout: post
title: "File permission trong Linux"
categories: misc
tags:
  - lorem
  - ipsum
---


## Mở bài [Có thể bỏ qua]

Phần lớn thời gian tại Đại học, tôi Được học trên nền tảng window với bộ công cụ visual studio và ms sql server. Mọi thứ Đều ổn Đến khi ra trường và Đi làm. Môi trường làm việc không còn là window nữa mà chuyển sang gia Đình linux-unix. Rất nhiều thứ lạ lẫm, tuy nhiên như nhiều lập trình viên khác. Copy paste không có gì khó khăn với tôi.

Từ những tutorial trên mạng, với những dòng lệnh tôi không hiểu, nhưng Đâu thành vấn Đề gì? nó Đang chạy và vậy là Đã Đủ hạnh phúc cho 1 cậu lập trình viên thiếu kinh nghiệm mới ra trường rồi. Tuy nhiên việc phải làm những công việc lặp Đi lặp lại hàng ngày mà không thực sự hiểu chúng khiến chúng ta khó chịu. Chẳng hạn như những câu lệnh set permission chẳng hạn.

## Thân bài

### 1.File permission là gì?

Tương tự như ngôi nhà của bạn, bạn sống cùng gia Đình và thi thoảng có những vị khách ghé thăm. Có những vật dụng trong nhà mà bạn chỉ muốn riêng mình bạn có thể sử dụng, cũng có những Đồ vật khác, thứ mà chỉ bạn và gia Đình Được phép sử dụng, và cũng có những Đồ vật mà bạn, gia Đình và cả những vị khách Đều có thể sử dụng.

Tài nguyên trong unix tương tự như vậy, mọi thứ trong unix Đều là file. Vậy nên có những file quan trọng chỉ mình bạn có thể xài, cũng có những file mà bạn có thể chia sẻ thêm cho 1 nhóm nào Đó. Còn những thứ không quá quan trọng bạn có thể mở cửa cho ai cũng có thể sử dụng. 

=> Đó là lý do permission cần tồn tại. 

### 2.Sử dụng là sử dụng thế nào?

Hiểu Đơn giản thế này, nhà của bạn, bạn cho tôi vào và cho phép tôi ngồi ghế uống nước xem tivi, nhưng không Đồng nghĩa với việc tôi muốn làm gì cái ghê hay cái tivi cũng Được. Tôi chỉ có thể sử dụng chứ không Được phép thay Đổi hay Đập phá Đồ Đạc của bạn. 

Trong unix có 3 quyền sử dụng: 

* Quyền Đọc.
* Quyền ghi.
* Quyền thực thi.

### 3.Các loại người dùng.

* Owner (người sở hữu file Đó - người tạo ra file Đó).
* Group (nhóm sở hữu file).
* Other (phần còn lại của hệ thống có thể là guest).

> Ngoài ra trên unix thì còn 1 người dùng thứ 4 gọi là root. Đây là người dùng siêu quyền lực và không thể bị ảnh hưởng bởi mấy lệnh phân quyền của bạn

Hiểu Đơn giản như việc bạn ở nhà của mình và nghĩ bạn có thể làm mọi việc với căn nhà Đó mà không ai có thể can thiệp. Tuy nhiên khi mở sổ Đỏ ra thì bạn sẽ thấy là bạn Đang có quyền sử dụng Đất thôi chứ không phải sở hữu Đất. Mọi Đất Đai Đều thuộc quyền sở hữu của nhà nước.

Đây chính là root trong trường hợp này, và nếu nhà nước thích thì quyền sở hữu nhà hay mọi tài nguyên trong nhà của bạn bay màu.

> Vì vậy hãy thật cẩn thận khi sử dụng quyền root trên máy tính. Trong trường hợp máy tính, thì chúng ta là chủ sở hữu nên chúng ta có thể enable hay disable root Đi. Trên Mac thì mặc Định là disable. 

### 4.Cách phân quyền

`chmod xyz file/folder` 

Ví dụ:  `chmod 007 duychinh.bin`

Giải thích câu lệnh trên: 

-xyz là 3 số nguyên liền nhau( giá trị của ba số này có thể là 0,1,2,3,4,5,6,7).

Với x ứng với quyền phân cho owner, y ứng với quyền phân cho group và z là cho everyone. 

0: không có quyền gì cả.

1: quyền thực thi tập tin.

2: quyền write only.

4: quyền read only.

Với 4 số trên ta có thể cộng lại và ra 1 số mới với permission Được cộng dồn. 

Ví dụ: 7 = 4 + 2 + 1 => tất cả các quyền. Tương tự với cả tổ hợp khác.

Suy ra với lệnh chmod 777 file_name 

=> Ta Đã gán tất cả quyền cho cả 3 loại user(như liệt kê ở mục 3).

Hình ảnh minh hoạ: 

![Branching](http://www.macinstruct.com/images/permissions/permissions1.png)

Trong nhiều trường hợp Để có thể thực hiện Được lệnh set permission kể trên, ta phải thêm câu lệnh `sudo`
vào phía trước. Trong các bài tiếp theo sẽ giải thích về ý nghĩa của câu lệnh này.

### 5.Tham khảo

https://www.guru99.com/file-permissions.html

# Kết bài

Trên là 1 cái nhìn tổng quan về phân quyền trên linux. Thế giới unix còn rất nhiều Điều thú vị, hi vọng trong thời gian tôi có thể có thời gian Để tìm hiểu sâu sắc hơn về nền tảng này.