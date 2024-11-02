# Apache Kafka là gì ?

Apache Kafka là hệ thống phân tán mã nguồn mở bao gồm máy chủ và máy khách được sử dụng chủ yếu để xây dựng các data streaming pipeline thời gian thực.

## Thách thức tích hợp dữ liệu

Các tổ chức thường có nhiều nguồn dữ liệu với định dạng khác nhau, từ các ứng dụng như kế toán, thanh toán, CRM, v.v. Việc tích hợp dữ liệu từ các nguồn này gặp nhiều thách thức như giao thức truyền tải, định dạng và lược đồ dữ liệu, dễ dẫn đến sự phức tạp.

Apache Kafka giúp đơn giản hóa quá trình này bằng cách đóng vai trò lớp tích hợp dữ liệu. Các nguồn dữ liệu gửi dữ liệu đến Kafka, và các hệ thống đích lấy dữ liệu từ đó, giúp tách rời và giảm phức tạp trong tích hợp dữ liệu.

![thách thức tích hợp dữ liệu](/images/kafka-01.png)

### Data Stream trong kafka là gì ?

Trong Apache Kafka, data streaming là một chuỗi dữ liệu không giới hạn, có thể truy cập ngay khi được tạo. Mỗi ứng dụng trong tổ chức có thể tạo ra một data streaming riêng với lưu lượng biến động, từ hàng nghìn bản ghi mỗi giây đến chỉ một vài bản ghi mỗi giờ.

Apache Kafka lưu trữ các data streaming này dưới dạng topics, cho phép xử lý dòng – tức là xử lý dữ liệu liên tục và theo thời gian thực. Sau khi được xử lý và lưu trong Kafka, dữ liệu có thể được chuyển sang các hệ thống khác như cơ sở dữ liệu. Kafka giúp tạo ra một đường dẫn dữ liệu theo thời gian thực, có khả năng xử lý hàng triệu sự kiện mỗi giây từ nhiều ứng dụng kinh doanh khác nhau.

![What is a data stream in Apache Kafka](/images/kafka-02.png)

## Ví dụ về Data Stream

Phân tích nhật ký (Log Analysis): Các ứng dụng hiện đại gồm hàng chục đến hàng nghìn microservices, tất cả đều tạo ra nhật ký (logs). Nhật ký chứa nhiều thông tin có thể khai thác cho phân tích kinh doanh, dự đoán lỗi và gỡ lỗi. Các công ty đưa dữ liệu nhật ký vào một dòng dữ liệu để xử lý liên tục.

Phân tích web (Web Analytics): Các ứng dụng web hiện đại theo dõi hầu hết các hoạt động của người dùng, như nhấp chuột và lượt xem trang. Các hoạt động này tăng lên nhanh chóng. Xử lý dòng dữ liệu giúp các công ty xử lý dữ liệu ngay khi được tạo ra thay vì chờ hàng giờ sau đó.
