# Zookeeper with Kafka

**Vai trò của Zookeeper trong Kafka**

Zookeeper đóng vai trò quan trọng trong việc vận hành cụm Kafka, chủ yếu là quản lý **medadata** và **điều phối cụm**. Dưới đây là các chức năng chính của Zookeeper trong Kafka:

1. **Theo dõi thành viên cụm**: Zookeeper giữ thông tin về tất cả các broker trong cụm Kafka, đảm bảo rằng các broker đều biết về sự tồn tại của nhau và có thể liên lạc với nhau.

2. **Bầu chọn leader**: Đối với mỗi partition của một topic trong Kafka, Zookeeper hỗ trợ chọn **broker dẫn đầu (leader)**. Khi broker leader của một partition bị lỗi, Zookeeper sẽ kích hoạt quá trình bầu chọn một leader mới trong số các bản sao (replica), đảm bảo tính liên tục của dữ liệu.

3. **Quản lý cấu hình và thông báo**: Zookeeper lưu trữ các cấu hình cho topic và quản lý quyền truy cập. Nó cũng gửi thông báo đến Kafka khi có các sự kiện như thêm hoặc xóa topic, broker mới, giúp Kafka phản hồi kịp thời.

4. **Offset của Consumer**: Mặc dù các offset của consumer trước đây được lưu trong Zookeeper, từ Kafka phiên bản 0.10 trở đi, offset đã được lưu trực tiếp trong Kafka, không còn yêu cầu kết nối trực tiếp tới Zookeeper.

![Zookeeper in Kafka](/images/kafka-31.png)

**Sự chuyển đổi của Kafka không dùng Zookeeper**

Hiện tại, Kafka đang loại bỏ dần sự phụ thuộc vào Zookeeper. Kể từ phiên bản Kafka 3.x, người dùng có thể chọn chạy Kafka mà không cần Zookeeper (theo KIP-500), nhưng tính năng này chưa sẵn sàng cho môi trường sản xuất. Đến Kafka 4.x, dự kiến Zookeeper sẽ được loại bỏ hoàn toàn.

Điều này có nghĩa là:

- **Kafka Brokers**: Trong Kafka 3.x trở đi, các broker có thể hoạt động mà không cần Zookeeper, mặc dù vẫn khuyến nghị dùng Zookeeper trong các triển khai sản xuất hiện tại.
- **Kafka Clients**: Các client và CLI của Kafka đã được điều chỉnh để kết nối trực tiếp với các broker thay vì Zookeeper. Chẳng hạn, từ Kafka 2.2, lệnh `kafka-topics.sh` sử dụng broker thay vì Zookeeper để quản lý topic, và tham số Zookeeper trong các lệnh CLI cũng bị loại bỏ dần.

**Lưu ý bảo mật**: Zookeeper kém bảo mật hơn Kafka, do đó chỉ nên mở cổng Zookeeper để nhận lưu lượng từ Kafka broker và không nên kết nối từ Kafka clients.

Tóm lại, để phù hợp với Kafka hiện đại, các nhà phát triển không cần và không nên sử dụng Zookeeper trong các cấu hình của Kafka client hoặc các chương trình kết nối đến Kafka.
