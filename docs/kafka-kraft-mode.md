# Kafka KRaft Mode

**Kafka KRaft Mode - Kafka không cần Zookeeper**

Từ năm 2019, dự án Apache Kafka đã thực hiện thay đổi lớn với KIP-500, nhằm loại bỏ Zookeeper như một thành phần phụ thuộc để vận hành Kafka. Cụ thể, Kafka đã phát triển **KRaft Mode** để thay thế Zookeeper, cho phép Kafka tự quản lý metadata và kiểm soát cụm của mình mà không cần Zookeeper.

### Tại sao Kafka lại loại bỏ Zookeeper?

Sự phụ thuộc vào Zookeeper đã gây ra một số hạn chế khi Kafka phải mở rộng quy mô, bao gồm:

1. **Giới hạn số phân vùng**: Cụm Kafka với Zookeeper chỉ hỗ trợ tối đa 200,000 phân vùng.
2. **Quá tải khi bầu chọn leader**: Khi có broker mới tham gia hoặc rời khỏi cụm, Zookeeper phải xử lý một lượng lớn các yêu cầu bầu chọn leader, dẫn đến khả năng quá tải và giảm hiệu suất.
3. **Khó khăn trong việc cài đặt và quản lý**: Việc thiết lập cụm Kafka trở nên phức tạp vì cần phụ thuộc vào Zookeeper.
4. **Thiếu đồng bộ trong metadata**: metadata của Kafka đôi khi không đồng bộ với Zookeeper.
5. **Bảo mật không nhất quán**: Zookeeper không đạt mức bảo mật cao như Kafka.

### Kafka KRaft Mode - Quản lý metadata không cần Zookeeper

KRaft (Kafka Raft) tận dụng metadata Kafka như một log để lưu trữ, cho phép các broker Kafka tự quản lý dữ liệu mà không cần Zookeeper. KRaft sử dụng **Raft protocol** để thực hiện các cuộc bầu chọn controller, mang lại các lợi ích chính sau:

- **Khả năng mở rộng tốt hơn**: Hỗ trợ hàng triệu phân vùng và dễ dàng duy trì, thiết lập.
- **Ổn định cao hơn**: Việc giám sát, hỗ trợ và quản lý cụm Kafka trở nên đơn giản hơn.
- **Bắt đầu một quy trình duy nhất**: Chỉ cần một quy trình để khởi động Kafka, thay vì khởi động cả Kafka và Zookeeper.
- **Mô hình bảo mật nhất quán**: Cả hệ thống Kafka được bảo mật theo một mô hình thống nhất.
- **Tốc độ phản hồi cao hơn**: Thời gian tắt và khôi phục controller được cải thiện.

### Triển khai KRaft

Kafka KRaft đã chính thức sẵn sàng cho sản xuất từ phiên bản Kafka 3.3, cung cấp khả năng ổn định và bảo mật hơn cho các cụm Kafka lớn.
