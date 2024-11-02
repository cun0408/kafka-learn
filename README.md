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

### Ví dụ về Data Stream

Phân tích nhật ký (Log Analysis): Các ứng dụng hiện đại gồm hàng chục đến hàng nghìn microservices, tất cả đều tạo ra nhật ký (logs). Nhật ký chứa nhiều thông tin có thể khai thác cho phân tích kinh doanh, dự đoán lỗi và gỡ lỗi. Các công ty đưa dữ liệu nhật ký vào một dòng dữ liệu để xử lý liên tục.

Phân tích web (Web Analytics): Các ứng dụng web hiện đại theo dõi hầu hết các hoạt động của người dùng, như nhấp chuột và lượt xem trang. Các hoạt động này tăng lên nhanh chóng. Xử lý dòng dữ liệu giúp các công ty xử lý dữ liệu ngay khi được tạo ra thay vì chờ hàng giờ sau đó.

### Apache Kafka có nhiều trường hợp sử dụng, bao gồm:

Hệ thống nhắn tin (Messaging Systems)
Theo dõi hoạt động (Activity Tracking)
Thu thập số liệu từ nhiều nguồn: Chẳng hạn, thu thập dữ liệu từ các thiết bị IoT phân tán.
Phân tích nhật ký ứng dụng (Application Logs Analysis): Thu thập và phân tích nhật ký từ các ứng dụng để giám sát và gỡ lỗi.
Tách rời các phụ thuộc hệ thống: Kafka cho phép các hệ thống không phụ thuộc trực tiếp vào nhau, tăng khả năng mở rộng và bảo trì.
Tích hợp với công nghệ Big Data: Kết hợp với Spark, Flink, Storm, Hadoop để xử lý và phân tích dữ liệu lớn.
Kho lưu trữ dựa trên sự kiện (Event Sourcing Store): Lưu trữ các sự kiện để xử lý sau này.
Apache Kafka là nền tảng lưu trữ quan trọng cho nhiều framework xử lý dòng dữ liệu như Apache Flink và Samza.

### Apache Kafka không phải là lựa chọn lý tưởng cho một số trường hợp sau:

Proxy cho hàng triệu thiết bị di động hoặc IoT: Giao thức Kafka không được thiết kế để phục vụ trực tiếp lượng lớn các thiết bị, mặc dù có một số proxy giúp kết nối.
Cơ sở dữ liệu với chỉ mục: Kafka là hệ thống nhật ký sự kiện, không có khả năng phân tích tích hợp sẵn hoặc mô hình truy vấn phức tạp như một cơ sở dữ liệu.
Công nghệ nhúng thời gian thực cho IoT: Các hệ thống nhúng có thể sử dụng những giải pháp nhẹ và cấp thấp hơn phù hợp hơn Kafka.
Hàng đợi công việc (Work Queues): Kafka sử dụng topics chứ không phải hàng đợi (queues) như RabbitMQ hay SQS. Kafka không tự động xóa dữ liệu sau khi xử lý, và số lượng consumer không thể vượt quá số phân vùng trong một topic.
Blockchain: Kafka có một số đặc điểm giống blockchain (dữ liệu có thể là bất biến), nhưng thiếu các tính năng quan trọng như xác minh mật mã và bảo toàn bộ lịch sử.

## Định nghĩa các khái niệm cốt lõi của Apache Kafka

![producer, topic and consumer](/images/kafka-03.png)

### Kafka Topic

Trong Apache Kafka, topic giúp tổ chức các sự kiện liên quan. Ví dụ, một topic có tên "logs" có thể chứa các bản ghi (logs) từ một ứng dụng. topic có thể xem tương tự như các bảng trong SQL, nhưng không thể truy vấn trực tiếp. Thay vào đó, cần tạo các Kafka producer và consumer để sử dụng dữ liệu.

Dữ liệu trong các topic được lưu trữ dưới dạng cặp khóa-giá trị (key-value) và ở định dạng nhị phân.

Tìm hiểu thêm tại [Kafka Topics, Partitions & Offsets](/docs/kafka-topics.md)
