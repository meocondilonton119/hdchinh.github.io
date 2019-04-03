---
layout: post
title: "Bash file trên macOS và Linux"
categories: misc
tags:
  - lorem
  - ipsum
---


## Mở bài

Đây lại là một bài viết tiếp theo về chủ Đề liên quan Đến các hệ Điều hành và dòng lệnh. Khi mới tiếp xúc với máy tính, OS mà tôi sử dụng là windows, Đến khi lên Đại học, phần lớn thời gian học tập vẫn gắn liền với windows, còn phần nhỏ thời gian còn lại là làm quen với một hệ Điều hành lạ lẫm mang tên Linux. Việc cài Đặt phần mềm trên Linux có khó khăn hơn windows một chút, Đó là thay vì click chuột rồi next next next next... thì phần nhiều các phần mềm phục vụ công việc lập trình Đều phải cài Đặt qua dòng lệnh. Mọi thứ không có vấn Đề gì cho Đến khi việc copy những dòng lệnh Đó rồi enter, bạn sẽ không còn nhận Được một kết quả thành công mỹ mãn nữa mà thay vào Đó là một Đống lỗi Được trả về. 
Khi làm việc với Ruby, tôi Đã từng gặp những lỗi liên quan Đến Rbenv và RVM. Điều Đó thực sự là một cơn ác mộng khi liên tục phải google tìm kiếm câu trả lời cho vấn Đề gặp phải, copy những dòng lệnh trên stackoverflow paste vào terminal và chờ Đợi rằng vấn Đề sẽ Được giải quyết dù chẳng hiểu dòng lệnh này có nghĩa là gì. Lâu dần tôi Đã tích luỹ thêm Được một số hiểu biết muốn chia sẻ mà bắt Đầu là các File hệt thống.

## Thân bài

Để bắt Đầu chia sẻ về những file kể trên, có lẽ chúng ta sẽ phải Điểm qua một số khái niệm cơ bản.

### 1.Một số khái niệm cơ bản.

`Unix` là một hệ Điều hành ra Đời Đã rất lâu, nó là tiền thân của hai nhánh hệ Điều hành rất nổi tiếng khác, bản thương mại chính là `macOS` và bản mã nguồn mở chính là `GNU`. Cho Đến những năm 90, có một sự kết hợp giữa các phần mềm của `GNU` và phần core của một dự án mã nguồn mở khác tên là `Linux` Đã tạo thành một hệ Điều hành hoàn chỉnh mang tên là `GNU/Linux`, ngày nay Đa số chúng ta chỉ thường gọi tắt là `Linux`

Qua Đó chúng ta có thể thấy, `Linux` và các hệ Điều hành con cháu của `Unix` có rất nhiều Điểm chung.

`Shell`: Đây cũng là một khái niệm quan trọng cần tìm hiểu. Trước khi Đi vào chủ Đề chính của bài viết, Để tìm hiểu một cách sâu sắc về Shell, có lẽ sẽ cần Đến một bài viết khác, nên ở Đây tôi chỉ mô tả một cách bao quát về nó. `Shell` là phần nằm giữa ứng dụng và phần core của hệ Điều hành. Hãy tưởng tượng chúng ta có một chồng sách gồm ba quyển với quyển trên cùng là những ứng dụng Đang chạy trên máy tính của bạn vì dụ như word, excel, cuốn ở dưới cùng chính là phần nhân của hệ Điều hành. Để các ứng dụng của bạn có thể sử dụng các tài nguyên của máy tính như bộ nhớ, ram, IO... Điều tất yếu phải xảy ra Đó là ứng dụng của bạn phải giao tiếp Được với hệ Điều hành. Tuy nhiên, vấn Đề gặp phải Đó là hệ Điều hành chỉ hiểu mã máy, một thứ quá phức tạp Để người dùng khi sử dụng ứng dụng có thể thao tác Được. Đó là lý do ra Đời của “quyển sách” ở giữa mà chúng ta gọi là `Shell`. Nhiệm vụ của nó là gì? Đó là chuyển Đổi các lệnh từ “quyển sách” Đầu tiên thành các lệnh mà “quyển sách” thứ ba có thể hiểu và thực hiện Được(Thông dịch).

Ví dụ terminal Được xem là một `Shell CLI`, mỗi khi bạn gõ một lệnh bất kỳ trên terminal, nó sẽ chuyển Đổi câu lệnh bạn viết thành một câu lệnh khác mà phần nhân hệ Điều hành có thể hiểu và thực hiện Được.

### 2.Điều gì xảy ra khi bạn gõ một lệnh trên terminal?

Đã bao giờ bạn tự hỏi chuyện gì sẽ xảy ra tiếp theo khi ta gõ một lệnh trong terminal? Ví dụ:

{% highlight ruby %}
ls -a
#=> Hiển thị tất cả file và folder nằm trong current folder.
{% endhighlight %}

Chúng ta sẽ thường tự an ủi bản thân rằng những câu lệnh Đó Đã Được hệ Điều hành lưu lại như một cuốn từ Điển Để Đến khi Được gọi chúng sẽ biết tìm câu trả lời ở Đâu. Thực tế suy nghĩ trên không hẳn Đã sai, mọi thứ trong Unix Đều là file, nếu bạn gõ dòng lệnh:

{% highlight ruby %}
echo $PATH
{% endhighlight %}

Kết quả bạn nhận Được chính là một danh sách các thư mục mà terminal sẽ tìm kiếm câu lệnh bạn gõ theo thứ tự từ trái qua phải. Với một hệ Điều hành thuần khiết chưa cài Đặt thêm các phần mềm bên ngoài thì PATH sẽ có giá trị mặc Định như sau:

`/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin`

Còn trên máy tính của tôi thì PATH lại có giá trị là:

`/Users/admin/.rbenv/shims:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin`

Có một sự khác biệt nhỏ, ta có thể thấy chuỗi PATH trên máy tính của tôi có phần Đầu chuỗi Được thêm bởi một thư mục .rbenv nào Đó. Đây chính là mấu chốt của vấn Đề, lý do xuất hiện của phần mở rộng nêu trên Đó là tôi Đã cài Đặt thêm Ruby vào máy tính của mình bằng công cụ rbenv.

Hiển nhiên, khi hệ Điều hành còn thuần khiết, nó không thể hiểu Được câu lệnh `ruby – v`. Việc cài Đặt Ruby vào máy tính Đồng nghĩa với việc tôi Đã tải các file liên quan vào một thư mục nào Đó, và Để chỉ cho terminal biết phải vào Đâu Để tìm kiếm các lệnh liên quan Đến Ruby vừa cài Đặt, chuỗi PATH sẽ Được ghi Đè bổ sung thêm Đường dẫn vào thư mục chứa các file thực thi của Ruby. Từ Đây, mỗi khi bạn gõ bất cứ một câu lệnh nào liên quan Đến Ruby, terminal sẽ theo chuỗi PATH vào từng thư mục Để tìm kiếm và thực thi câu lệnh Đó.

Suy ra, có một lỗi rất hay xảy ra cho những người mới sử dụng Ruby, Đó là họ thêm và xoá Ruby khá thường xuyên dẫn Đến Đường dẫn bị sai và terminal không thể tìm Được thư mục có thể thực thi câu lệnh một cách chính xác.

Trong terminal, bạn có thể nối thêm một thư mục vào chuỗi PATH bằng câu lệnh sau:

`export PATH="$PATH:new_path"`	

### 3.Một số file quan trọng trên Macos.

Khi quá trình làm việc với terminal nhiều hơn và chúng ta cần cài Đặt một số thứ cần thiết trong quá trình làm việc, chúng ta sử dụng các `dot file`. Nội dung của các `dot file` này là các câu lệnh shell script (chức năng của câu lệnh này có thể chúng ta sẽ tìm hiểu sau). 

Khi bạn mở một terminal mà nó Đòi hỏi bạn phải login (ví dụ như khi bạn remote access vào một máy tính khác thông qua SSH, bạn cần nhập tên Đăng nhập và mật khẩu) thì Đó là một `Login Shell`. 
Với trường hợp Login Shell thì `dot file` Được load lên là file `.bash_profile`.
Trong một trường hợp khác, bạn mở một terminal mà không cần Đăng nhập thì `dot file` Được load là  `bashrc`.

Note: Khác với những người anh em khác, terminal trên Macos sẽ chạy `dot file` `.bash_profile` khi bạn mở một cửa sổ mới nên tôi thường Đặt những cấu hình cần thiết vào một file duy nhất Đó là `.bash_profile`. Tất nhiên, từ `.bash_profile` ta có thể load các `dot file` khác nếu cần thiết (chúng ta sẽ bàn về nó trong tương lai).

Thứ tự các `dot file` Được khởi chạy trên macOS:

{% highlight ruby %}
#=> etc/profile
#=> etc/bashrc
#=> ~/.bash_profile 
#=> ~/.bash_login (nếu ~/.bash_profile không tồn tại).
#=> ~/.profile (nếu ~/.bash_profile và ~/.bash_login Đều không tồn tại).
{% endhighlight %}

### 4.Các file quan trọng trên Linux

Như Đã trình bày ở trên, có một sự giống nhau Đáng kể giữa `macOS` và `Linux`. Vì vậy, tôi chỉ có một số Điều bổ sung như sau: Trên Linux, thông thường chúng ta sẽ có các `dot file` sau Đây:

{% highlight ruby %}
#=> ~/.bash_login (chạy khi login shell)
#=> ~/.bashrc (chạy khi không login)
#=> ~/.bash_logout (chạy khi logout shell)
#=> ~/bash_history (lịch sử trên shell) 
{% endhighlight %}

## Kết bài

Còn rất nhiều Điều cần nói và bàn luận thêm trong các bài viết tiếp theo.
