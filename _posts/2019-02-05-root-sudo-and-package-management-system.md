---
layout: post
title: "Root, Sudo và Package Management"
categories: misc
tags:
  - lorem
  - ipsum
---


## Mở bài

Trong một bài viết gần Đây về chủ Đề file permission trên Unix/Linux, tôi có Đề cập Đến một người dùng quyền lực trên các hệ thống kể trên có tên gọi là `root`. Bài viết này sẽ khái quát về người dùng này kèm theo hai khái niệm có liên quan là `sudo` và `Package Management System`.

## Thân bài

### 1.Root

Xin Được nhắc lại một câu chuyện cũ tôi Đã kể trong bài viết trước với ví dụ về mảnh Đất nhà bạn, bạn sở hữu mảnh Đất Đó, vợ bạn sở hữu mảnh Đất Đó chung với bạn. Nhưng vợ chồng bạn không phải là những người sở hữu tối thượng của mảnh Đất kể trên, mà quyền lực to lớn này thuộc về nhà nước, “người” thực ra là Đang cho bạn thuê Đất có thời hạn. Tương tự trọng Unix/Linux, sẽ có những user (như tài khoản bạn sử dụng Để Đăng nhập, hoặc tài khoản khách…). 

Việc những tài khoản này có thể Đăng nhập vào hệ thống nhưng Đặc quyền của chúng bị giới hạn. Khi login vào hệ thống, bạn có thể làm một số tác vụ nhất Định chứ không thể thích làm gì thì làm, trong hệ thống của bạn chỉ có một user duy nhất Được quyền làm bất cứ Điều gì, Đó là một super user. 

Trên windows, ta gọi Đó là `administrator` (có lẽ quá quen thuộc, bạn còn nhớ những lúc cài phần mềm, bạn click vào file exe nhưng chương trình không chịu cài Đặt, google một lát, bạn nhận Được một hướng dẫn hãy click phải vào file exe Đó và chọn `run as administrator`, Đó chính là sức mạnh của super user trên windows), còn trên macOS hay Ubuntu(1 Đại diện của Linux), nó Được gọi là `root` (cuối cùng cũng chỉ là một cái tên gọi, nó có thể Được Đặt tên là mèo, là chó, nó có thể Được Đặt tên là bất cứ thứ gì, chúng ta không cần quan tâm, Điều duy nhất chúng ta cần biết Đó là trên các hệ Điều hành con cháu của Unix sẽ luôn có một super user quyền lực như vậy).

Đến lúc này, bạn sẽ tự hỏi tại sao phải thiết kế hệ thống phức tạp như vậy? Tại sao không chỉ dung duy nhất một loại user? 
Câu trả lời rất Đơn giản, không phải ai sử dụng máy tính cũng là những người dung có hiểu biết sâu sắc về hệ Điều hành. Vì vậy, nếu trao cho họ những quyền lực quá lớn, họ có thể vô tình gây ra những huỷ hoại nghiêm trọng cho hệ thống (quyền lực càng lớn thì trách nhiệm càng lớn).

Một vấn Đề phát sinh nếu việc sử dụng Root mang tính rủi ro quá cao nhưng những user bình thường lại không có các quyền Để cài Đặt một số phần mềm nâng cao thì phải làm sao? Vấn Đề này sẽ Được giải thích ở mục số 2.

Note: Trên macOS và Ubuntu, `root` mặc Định sẽ Được disable, chúng có thể sẽ Được enable trở lại và re-disable. Cách thực hiện thì có lẽ bạn nên tìm kiếm hướng dẫn từ trang chủ của những hệ Điều hành này Để câu lệnh của bạn Đảm bảo chính xác và không bị lỗi thời.

### 2.Sudo

Như vấn Đề tôi Đã nêu ở cuối mục 1, bây giờ tôi muốn cài Đặt một số phần mềm Đòi hỏi quyền của super user, về lý thuyết tôi Đứng giữa hai lựa chọn hoặc chuyển sang `root` và cài Đặt phần mềm bình thường (rất rủi ro), phương án 2 là tôi vẫn sử dụng user thường và nói lời tạm biệt với những phần mềm kể trên. 

Quá tệ, cả hai giải pháp trên Đều không ổn, thật may, các nhà phát triển hệ thống Đã cung cấp cho chúng ta một lựa chọn số 3, Đó là bạn vẫn sử dụng user thường nhưng có thể triệu hồi quyền lực của super user Để thực hiện những lệnh nào Đó.

Cách sử dụng Đó là bạn thêm câu lệnh `sudo` vào phía trước câu lệnh cần sử dụng quyền của super user. Ví dụ:

`sudo apt-get update`

Một nhầm lẫn thường xuyên với cả với tôi ngày mới tìm hiểu, Đó là sau khi gõ những câu lệnh có `sudo`, hệ thống yêu cầu tôi nhập mật khẩu và tôi Đã từng nghĩ Đó là mật khẩu của `root` (nghe cũng hợp lý Đấy chứ, ta mượn quyền của `root` thì phải sử dụng mật khẩu của `root` chứ nhỉ? Giống như khi tôi mượn nhà của bạn thì tôi phải có chìa khoá của bạn). Nhưng không phải như vậy, mật khẩu bạn nhập vào Đơn thuần là chính mật khẩu của user thường bạn Đang login trên máy.

Phần Đa các trường hợp khi bạn thêm `sudo`, các câu lệnh sẽ chạy một cách hoàn hảo. Tuy nhiên, cũng có những khi bạn không nhận Được kết quả như mong Đợi và lý do chính là không phải user nào cũng có thể triệu hồi quyền lực của super user. Điều bạn cần làm khi Đó là "nói" với hệ Điều hành rằng hãy cho user của tôi Đang sử dụng Được quyền triệu hồi sức mạnh của super user. Hãy xem ví dụ dưới Đây:

{% highlight ruby %}
#=> /etc/sudoers (lưu trữ các user Được quyền thực hiện Đặc quyền của super user)
#=> Ví dụ
root ALL=(ALL:ALL) ALL
#=> user Đang xét quyền tên là root
#=> Chữ all Đầu tiên chỉ ra rằng rule này Được áp dụng cho tất cả các host
#=> Chữ all thứ hai chỉ ra, account root có thể chạy lệnh với quyền của bất kỳ user nào.
#=> Chữ all thứ ba chỉ ra, account root có thể chạy lệnh với quyền của bất kỳ group nào.
#=> Chữ all cuối cùng chỉ ra, account root có thể chạy bất kỳ command nào.
{% endhighlight %}

Note: Kể từ khi bạn nhập mật khẩu sau khi gõ lệnh `sudo` lần Đầu, thời gian có hiệu lực của mật khẩu này sẽ là 15 phút. Trong khoảng thời gian Đó bạn có thể chạy các lệnh `sudo` khác mà hệ Điều hành không yêu cầu bạn nhập lại mật khẩu.


### 3.Package management system

Khi sử dụng macOS, Để cài Đặt thêm các phần mềm phục vụ công việc (mà chúng không Được support trên appstore), tôi sử dụng `homebrew`, một phần mềm giúp quản lý các phần mềm khác rất hiệu quả.

Vậy còn trên Linux chúng ta có gì?

Trước hết cần phải làm rõ: Package management system là một công cụ hoặc một hệ thống các công cụ Được sử dụng Để handle các package.
1.	Handle là quản lý với Đầy Đủ các chức năng như thêm mới, gỡ bỏ, nâng cấp, tìm kiếm..v..v… 
2.	Package là một tiện ích hoặc những phần bổ trợ cho một tiện ích nào Đó (thực sự cuối cùng nó cũng chỉ là tập hợp của các file).

Một `Package Management System` nổi tiếng trên Linux Đó là `DPKG`, nó là một công cụ Để cài Đặt các package có Đuôi .deb (các package dành cho các hệ Điều hành thuộc dòng debian).
Nhiệm vụ của `DPKG` là install hoặc remove một package (với Điều kiện các file cần thiết Đã có trên local).

Đến Đây nhiều bạn có thể thắc mắc rằng từ trước Đến giờ bạn chưa hề sử dụng các dòng lệnh liên quan Đến `DPKG`, vậy tại sao tôi lại bảo là nó rất phổ biến? Câu trả lời Đó là bạn Đang sử dụng các phần mềm package management system khác Được xây dựng dựa trên phần core là `DPKG`, một trong số Đó là `APT–GET/APT`.

Ưu Điểm lớn nhất của `APT` so với `DPKG` Đó là `DPKG` chỉ có thể cài Đặt file khi mà file Đó Đã có trên local (tức là bằng cách nào Đó bạn phải download hoặc copy file Đó về máy của bạn). Còn với `APT`, nó sẽ tìm kiếm các package Đó từ `remote repositories`, sau Đó download chúng về máy. 

Điểm mạnh thứ hai của `APT` so với `DPKG` Đó là nó giải quyết Được bài toán dependencies. Nếu package bạn cần cài Đặt phụ thuộc vào một vài package khác, nếu sử dụng `DPKG` thì bạn sẽ phải cài Đặt tất cả các package kể trên một cách thủ công.
Còn với `APT`, nó sẽ tự hiểu Được package Đó cần thêm những phụ trợ nào và tự Động cài Đặt phần phụ trợ.

Điều thật sự diễn ra Đó là trong `APT`, `DPKG` vẫn tồn tại. Nó tồn tại ở một cấp thấp hơn và thực hiện các chỉ thị của `APT`.

Note: Các `remote repositories` Để phần mềm package management của bạn tìm kiếm sẽ Được lưu trữ trong file `/etc/apt/sources.list` (Đa số là như vậy).

## Kết bài

Còn rất nhiều Điều cần nói và bàn luận thêm trong các bài viết tiếp theo.
