# Kafka Brokers

- **Kafka Broker** là một server chạy chương trình Kafka, thông thường được tối ưu hóa để chỉ chạy chương trình Kafka mà không có các dịch vụ khác. Một Kafka broker vận hành trên Java Virtual Machine (JVM), yêu cầu phiên bản Java từ 11 trở lên.
- **Kafka Cluster**: Một tập hợp các Kafka brokers được gọi là một Kafka cluster. Cluster này có thể chỉ có một broker, hoặc có thể có hàng trăm broker để xử lý lượng dữ liệu lớn như ở các công ty như Netflix và Uber. Một broker trong một cụm được xác định bằng một ID số duy nhất. Trong hình bên dưới, cụm Kafka được tạo thành từ ba broker Kafka.

![Kafka Cluster](/images/kafka-21.png)

### Kafka Brokers và Topics

- Các **partition** của một topic có thể được phân phối đều trên các broker trong một Kafka cluster. Điều này giúp tăng thông lượng (throughput) và khả năng mở rộng của hệ thống.
- Mỗi broker lưu trữ dữ liệu trong một thư mục riêng trên đĩa của nó. Với mỗi partition của topic, broker sẽ tạo ra một thư mục con riêng chứa dữ liệu cho partition đó.

Ví dụ: Một topic có thể có nhiều partition như Topic-A với ba partition. Các partition này sẽ được phân phối đều trên ba brokers. Một topic khác, như Topic-B, chỉ có hai partition nên có thể không cần phân phối hết các brokers, ví dụ Broker 103 sẽ không lưu bất kỳ partition nào của Topic-B.

![Kafka Topic Partitions](/images/kafka-22.png)

### Kết nối đến Kafka Cluster (Bootstrap Server)

- **Bootstrap Server**: Bất kỳ broker nào trong cluster đều có thể được sử dụng như một điểm bắt đầu kết nối (bootstrap server) vì mỗi broker đều lưu trữ thông tin về các brokers khác trong cluster. Khi một client kết nối với bất kỳ broker nào, broker đó sẽ cung cấp danh sách các brokers khác và các metadata cần thiết.
- **Kết nối nhiều Bootstrap Servers**: Trong thực tế, client thường kết nối đến ít nhất hai bootstrap servers để đảm bảo độ tin cậy, phòng trường hợp một broker không sẵn sàng. Việc này giúp các client không cần phải biết tất cả địa chỉ của mọi broker trong cluster.

Điều này giúp Kafka broker hỗ trợ việc **phân chia tải** và giúp **cân bằng hệ thống** một cách linh hoạt và đáng tin cậy.

![Connecting to a Kafka Cluster](/images/kafka-22.png)
