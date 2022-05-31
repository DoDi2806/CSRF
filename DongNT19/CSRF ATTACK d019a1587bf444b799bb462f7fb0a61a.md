# CSRF ATTACK

# I, CSRF là gì?

CSRF ( Cross Site Request Forgery) là kỹ thuật tấn công bằng cách sử dụng quyền chứng thực của người dùng đối với một website. CSRF là kỹ thuật tấn công vào người dùng, dựa vào đó hacker có thể thực thi những thao tác phải yêu cầu sự chứng thực. Hiểu một cách nôm na, đây là kỹ thuật tấn công dựa vào mượn quyền trái phép.

# II, Kịch bản tấn công

Các ứng dụng web hoạt động theo cơ chế nhận các câu lệnh HTTP từ người sử dụng, sau đó thực thi các câu lệnh này. Hacker sử dụng phương pháp CSRF để lừa trình duyệt của người dùng gửi đi các câu lệnh http đến các ứng dụng web. Điều đó có thể thực hiện bằng cách chèn mã độc hay link đến trang web mà người dùng đã được chứng thực. Trong trường hợp phiên làm việc của người dùng chưa hết hiệu lực thì các câu lệnh trên sẽ được thực hiện với quyền chứng thực của người sử dụng. Ta có thể xét ví dụ sau:

- DongNT19 truy cập vào tài khoản để chuyển tiền. Một người dùng khác, TienTV 22 đăng tải một bài viết vô cùng gợi cảm. Giả sử rằng TienTV22 có ý đồ không tốt và anh ta muốn ăn trộm tiền của DongNT19.
- TienTV22 sẽ tạo một trang web và chèn những đoạn mã code độc hại.

Để tăng hiệu quả che dấu, đoạn mã trên có thể được thêm các thông điệp bình thường để người dùng không chú ý và chỉ cần người dùng truy cập là form chuyển tiền sẽ tự động submit đến tài khoản attacker.

- DongNT19 đăng nhập vào trang web [**www.example.com**](http://www.example.com) để chuyển tiền và chưa thực hiện việc logout để kết thúc phiên làm viêc. DongNT19 lại xem được một bài có chứa hình ảnh nóng bỏng do TienTV22 đăng, trình duyệt sẽ đọc đoạn mã code độc hại để chuyển tiền đến tài khoản TienTV 22.

# III, Demo

- Xây dựng trang web có chức năng login để chuyển tiền.

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled.png)

- Khi truy cập vào đường dẫn [**https://0f1c-171-241-60-33.ap.ngrok.io/transfer](https://0f1c-171-241-60-33.ap.ngrok.io/transfer)** ứng dụng web sẽ yêu cầu người dùng login thì mới cho phép chuyển tiền.

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%201.png)

- Người dùng chuyển tiền bình thường.

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%202.png)

- Database đã cập nhật lịch sử chuyển tiền của dongnt.

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%203.png)

- Attacker xây dựng form chuyển tiền để lừa người dùng truy cập.
- Để tăng tính hiệu quả, attacker đã thêm một đoạn script “onload = "document.transferForm.submit()” để trình duyệt tự động submit khi người dùng truy cập

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%204.png)

- Sau khi người dùng truy cập, trình duyệt tự submit form chuyển tiền mà đến tài khoản của attacker:
- Account: 286
- Value: 28060504

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%205.png)

# IV, CÁCH PHÒNG CHỐNG

Dựa trên nguyên tắc của CSRF "lừa trình duyệt của người dùng (hoặc người dùng) gửi các câu lệnh HTTP", các kĩ thuật phòng tránh sẽ tập trung vào việc tìm cách phân biệt và hạn chế các câu lệnh giả mạo.

## 4.1 Phía user

Để phòng tránh trở thành nạn nhân của các cuộc tấn công CSRF, người dùng internet nên thực hiện một số lưu ý sau:

- Nên thoát khỏi các website quan trọng: Tài khoản ngân hàng, thanh toán trực tuyến, các mạng xã hội, gmail, yahoo… khi đã thực hiện xong giao dịch hay các công việc cần làm. (Check - email, checkin…)
- Không nên click vào các đường dẫn mà bạn nhận được qua email, qua facebook … Khi bạn đưa chuột qua 1 đường dẫn, phía dưới bên trái của trình duyệt thường có địa chỉ website đích, bạn nên lưu ý để đến đúng trang mình muốn.
- Không lưu các thông tin về mật khẩu tại trình duyệt của mình (không nên chọn các phương thức "đăng nhập lần sau", "lưu mật khẩu" …
- Trong quá trình thực hiện giao dịch hay vào các website quan trọng không nên vào các website khác, có thể chứa các mã khai thác của kẻ tấn công.

## 4.2 Phía server

Có nhiều lời khuyến cáo được đưa ra, tuy nhiên cho đến nay vẫn chưa có biện pháp nào có thể phòng chống triệt để CSRF. Sau đây là một vài kĩ thuật sử dụng.

### **4.2.1 Lựa chọn việc sử dụng GET VÀ POST**

Sử dụng GET và POST đúng cách. Dùng GET nếu thao tác là truy vấn dữ liệu. Dùng POST nếu các thao tác tạo ra sự thay đổi hệ thống (theo khuyến cáo của W3C tổ chức tạo ra chuẩn http) Nếu ứng dụng của bạn theo chuẩn RESTful, bạn có thể dùng thêm các HTTP verbs, như PATCH, PUThay DELETE

### **4.2.2 Sử dụng captcha, các thông báo xác nhận**

Captcha được sử dụng để nhận biết đối tượng đang thao tác với hệ thống là con người hay không? Các thao tác quan trọng như "đăng nhập" hay là "chuyển khoản" ,"thanh toán" thường là hay sử dụng captcha. Tuy nhiên, việc sử dụng captcha có thể gây khó khăn cho một vài đối tượng người dùng và làm họ khó chịu. Các thông báo xác nhận cũng thường được sử dụng, ví dụ như việc hiển thị một thông báo xác nhận "bạn có muốn xóa hay k" cũng làm hạn chế các kĩ thuật Cả hai cách trên vẫn có thể bị vượt qua nếu kẻ tấn công có một kịch bản hoàn hảo và kết hợp với lỗi XSS.

### **4.2.3 Sử dụng token**

Tạo ra một token tương ứng với mỗi form, token này sẽ là duy nhất đối với mỗi form và thường thì hàm tạo ra token này sẽ nhận đối số là"SESSION" hoặc được lưu thông tin trong SESSION. Khi nhận lệnh HTTP POST về, hệ thống sẽ thực hiên so khớp giá trị token này để quyết định có thực hiện hay không. Mặc định trong Rails, khi tạo ứng dụng mới:

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%206.png)

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%207.png)

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%208.png)

![Untitled](CSRF%20ATTACK%20d019a1587bf444b799bb462f7fb0a61a/Untitled%209.png)