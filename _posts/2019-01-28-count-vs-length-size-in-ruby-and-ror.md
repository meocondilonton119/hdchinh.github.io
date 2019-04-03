---
layout: post
title: "Count, length, size trong Ruby và Ruby On Rails"
categories: misc
tags:
  - lorem
  - ipsum
---


## Mở bài

Khoảng 1 năm trước, khi tôi bắt Đầu học về Ruby on Rails, giống như nhiều developers khác, tôi chủ yếu tập trung vào cách Để có thể xây dựng một ứng dụng ruby on rails chạy Được mà không tập trung nhiều vào kiến thức Ruby nền tảng. Có lẽ Đấy cũng là lựa chọn duy nhất vào thời Điểm Đó với một cậu sinh viên mới ra trường như tôi, khi mà có quá nhiều khái niệm, quá nhiều thứ phải học mà sếp của tôi cũng như sếp của bạn sẽ không trả lương cho chúng ta lên công ty Để “học”. 
Thời gian cứ trôi, khi có nhiều kinh nghiệm hơn, tôi bắt Đầu ngày càng hứng thú về những kiến thức nền tảng mà ngày xưa tôi chỉ từng Đọc lướt qua. Trong Đó có Count, Length và Size, những thứ tưởng như không có gì Để nói hay tìm hiểu thêm, nhưng liệu chúng có thực sự Đơn giản như vậy?

## Thân bài

### 1.Tổng quan

Về cơ bản, Đây là ba phương thức Được Ruby cung cấp, và dĩ nhiên, nó hoàn toàn có thể sử dụng trong rails. Một cách thường xuyên nhất, bạn có thể thấy người ta sử dụng những phương thức này cho một mảng dữ liệu bất kì trong Ruby hay một mảng record trong Rails. Ý nghĩa của nó là trả về kích thước của mảng/hash. Ví dụ:

{% highlight ruby %}
array = Array.new(3,1)
#=> Tạo 1 mảng tên array có 3 phần tử Đều có giá trị là 1.
array.size, array.length, array.count
#=> Kết quả trả về cùng là 3.
{% endhighlight %}

Nhưng mọi chuyện không dừng lại ở Đấy, nếu chúng chỉ cùng có chung một chức năng thì không có lý do gì Để chúng cùng tồn tại.

### 2.Trong ruby

Ngoài chức năng tính kích thước của một mảng hay một hash như Đã Được Đề cập ở trên, Size có thể Được sử dụng với cả Đối tượng dạng chuỗi và dạng số nguyên.
Với chuỗi, Size sẽ trả về kết quả là số lượng ký tự của chuỗi. Còn với số nguyên, nó sẽ trả về số bite mà Đối tượng Đó nắm giữ trên bộ nhớ. Ví dụ:

{% highlight ruby %}
str_temp = "hduychinh"
str_temp.size
#=> Kết quả trả về Độ dài chuỗi là 9.
int_temp = 12
int_temp.size
#=> Kết quả trả về là 8, ứng với 8 bytes mà int_temp lưu trữ trên bộ nhớ.
{% endhighlight %}

Length kém hơn size một chút, Length chỉ có thể sử dụng thêm cho các Đối tượng dạng chuỗi Để trả về số lượng ký tự trong chuỗi, mà không thể sử dụng trên số nguyên.

Cuối cùng là Count, nó không có chức năng gì bổ sung so với phần Đã Đề cập. Tuy nhiên, một Điểm mạnh của Count Đó là khi sử dụng với mảng/hash thì nó có thể nhận tham số chuyển vào. Ví dụ:

{% highlight ruby %}
c = [1, 2, 3, 3]
c.count 3
#=> Kết quả trả về là 2.
c.count { |i| i > 1 }
#=> Kết quả trả về là 3.
{% endhighlight %}

### 3.Trong Active Record

Khi sử dụng Count thực chất là bạn Đang sử dụng lệnh sql `select count(*) from table_names` Để truy vấn ra số lượng mà chúng ta Đang cần tìm hiểu. Thứ hai, Count không Được lưu trữ lại, Điều Đó có nghĩa mỗi lần bạn chạy hàm Count thì nó sẽ truy vấn vào cơ sở dữ liệu mà không hề quan tâm là bạn có gọi lệnh Count trước Đó hay không. Thứ ba, Count là lệnh duy nhất trong ba lệnh trên có thể gọi trực tiếp thông qua một model class, ví dụ như: `Cat.count`.

Khi sử dụng Length, Điều Đầu tiên Đó là load toàn bộ record mà bạn Đang Định Đếm số lượng vào bộ nhớ rồi tính toán. Từ lần gọi thứ hai, nó sẽ lấy lại kết quả cũ chứ không load dữ liệu lên nữa. Đây thực sự là một Điều tồi tệ nếu bạn có một cơ sở dữ liệu lớn, nó có thể load hàng triệu record vào bộ nhớ.

{% highlight ruby %}
Cat.all.length
#=> Cat Load (0.2ms)  SELECT "cats".* FROM "cats"
{% endhighlight %}

Khi sử dụng Size, Điều Đầu tiên của nó là tìm xem kết quả của truy vấn Đã có trong bộ nhớ hay chưa, nếu có rồi thì nó sẽ lấy ra và sử dụng, còn nếu chưa có, nó sẽ sử dụng một câu truy vấn SQL vào cơ sở dữ liệu Để Đếm số lượng. Điều này thật sự tuyệt vời, như một sự kết hợp giữa length và count.

## Kết bài

Theo dữ liệu từ fast-ruby thì method length chạy gần tương Đương với size và chạy nhanh hơn hăn so với count. Điểm lợi của count là nó có thể Được gọi từ 1 class Model, giúp cú pháp tường minh hơn, cũng như việc có thể truyền tham số vào hàm. Ngược lại trong các trường hợp khác, việc sử dụng size là khả dĩ hơn cả, Đặc biệt với dữ liệu lớn thì nên tránh sử dụng length, vì Đó có thể mang lại 1 thảm hoạ cho chương trình khi quá nhiều dữ liệu bị nạp vào bộ nhớ 1 cách vô ích.
