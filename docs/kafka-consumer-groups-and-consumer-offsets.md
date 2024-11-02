# Kafka Consumer Groups & Offsets

**Kafka Consumer Groups** và **Offsets** là hai thành phần quan trọng trong Apache Kafka giúp các ứng dụng xử lý dữ liệu theo quy mô lớn và đảm bảo tính liên tục của dữ liệu.

## Kafka Consumer Groups

- **Nhóm tiêu thụ (Consumer Group)**: Là một tập hợp các consumers cùng thực hiện một nhiệm vụ chung trong ứng dụng. Các consumer trong nhóm sẽ phân chia công việc để đọc dữ liệu từ các partition khác nhau của một topic.
- **partition và phân phối công việc**: Mỗi partition của topic chỉ được gán cho một consumer trong nhóm. Tuy nhiên, một consumer có thể đọc từ nhiều partition. Điều này giúp tối ưu hóa hiệu suất và giảm tải công việc.
- **ID nhóm (group.id)**: Để xác định một nhóm consumer trong Kafka, ta thiết lập thuộc tính `group.id` để Kafka biết rằng các consumer đó thuộc cùng một nhóm.

![Kafka Consumer group reading from topic 5 partition](/images/kafka-17.png)
![Kafka Consumer Groups](/images/kafka-18.png)

Khi số lượng consumer nhiều hơn số partition trong topic, một số consumer sẽ không hoạt động. Để tăng thông lượng, ta có thể tạo thêm nhiều partition khi thiết lập topic.

![More consumers than partitions](/images/kafka-19.png)

## Kafka Consumer Offsets

**Offset** là vị trí của một message trong partition Kafka, giúp theo dõi dữ liệu mà consumer đã đọc:

- Mỗi consumer group lưu lại thông tin về offset của các message đã được xử lý qua một topic nội bộ của Kafka là `__consumer_offsets`.
- Consumer tự động commit offset thường xuyên theo chu kỳ. Offset cuối cùng được commit là điểm mà consumer sẽ bắt đầu đọc lại nếu có lỗi hoặc cần khởi động lại.
- Các thư viện client Kafka thường tự động commit offset (với tùy chọn `enable.auto.commit=true`), giúp theo dõi quá trình xử lý mà không phải ghi vào `__consumer_offsets`.

![Consumer Offset](/images/kafka-20.png)

## Tại sao Offset quan trọng?

- **Duy trì trạng thái đọc**: Khi consumer gặp sự cố, các consumer còn lại có thể sử dụng offset để biết được điểm khởi đầu tiếp tục xử lý.
- **Điều phối nhóm (Rebalance)**: Khi một consumer mới tham gia hoặc rời khỏi nhóm, Kafka sẽ phân phối lại các partition và sử dụng offset đã commit để khởi động lại.

## Cơ chế giao nhận tin (Delivery Semantics)

Các cơ chế giao nhận tin kiểm soát cách mà Kafka commit offset:

1. **At most once**: Offset được commit ngay khi nhận message. Nếu có lỗi xảy ra, message sẽ bị mất và không được đọc lại.
2. **At least once** (phổ biến nhất): Offset được commit sau khi message được xử lý. Nếu có lỗi, message có thể được đọc lại, dẫn đến khả năng trùng lặp message. Để đảm bảo an toàn, việc xử lý nên là idempotent (xử lý nhiều lần không gây ra hiệu ứng không mong muốn).
3. **Exactly once**: Đảm bảo rằng mỗi message chỉ được xử lý một lần. Để đạt được điều này cho luồng Kafka-to-Kafka, cần dùng API giao dịch và cấu hình `processing.guarantee=exactly_once_v2` trong Kafka Streams API. Đối với luồng Kafka đến hệ thống bên ngoài, cần dùng consumer có tính chất idempotent.

Với các cơ chế này, "At least once" với xử lý idempotent là phương án thực tế và phổ biến nhất cho Kafka consumers, đảm bảo dữ liệu được xử lý liên tục và ổn định.
