# Kafka Topic Replication

**Kafka Topic Replication** là cơ chế quan trọng trong Apache Kafka nhằm đảm bảo tính bền vững và khả dụng của dữ liệu, ngay cả khi xảy ra sự cố ở các Kafka brokers.

### Kafka Topic Replication Factor

- **Replication Factor** là số lần một phần dữ liệu của topic được sao chép sang các broker khác nhau. Nó được xác định trong quá trình tạo topic.
- Nếu `replication factor` bằng 1, nghĩa là không có sao chép. Điều này thường chỉ dùng cho môi trường phát triển và không nên áp dụng cho môi trường sản xuất.
- Ví dụ, với `replication factor` bằng 3, dữ liệu sẽ được lưu trên ba broker khác nhau, giúp chống mất dữ liệu ngay cả khi có hai broker gặp sự cố.

  ![Kafka Topic Replication](/images/kafka-24.png)

### Kafka Partitions: Leader và Replica

- Mỗi **partition** của một topic sẽ có một broker được chỉ định là **leader**. Broker này chịu trách nhiệm xử lý dữ liệu từ và đến client.
- Các **replica** là các broker khác cũng lưu trữ bản sao của partition nhưng không giao tiếp trực tiếp với client. Nếu broker leader gặp sự cố, một replica sẽ được chọn làm leader mới.

### In-Sync Replicas (ISR)

- **ISR** là những replica cập nhật đầy đủ dữ liệu từ leader. Nếu một replica không thể theo kịp leader, nó sẽ bị đánh dấu là không đồng bộ.
- Khi một broker leader gặp sự cố, một replica trong ISR sẽ trở thành leader mới, đảm bảo dữ liệu không bị mất.

  ![Leaders & In-Sync Replicas](/images/kafka-25.png)

### Kafka Producer Acks Settings

- **acks** là mức độ xác nhận khi Kafka producer gửi dữ liệu đến broker:

  - `acks=0`: Producer không cần xác nhận từ broker. Cách này nhanh nhưng dễ mất dữ liệu khi xảy ra sự cố.

  ![acks = 0](/images/kafka-26.png)

  - `acks=1`: Producer chờ xác nhận từ leader. Có thể mất dữ liệu nếu leader gặp sự cố trước khi dữ liệu được sao chép.

  ![acks = 1](/images/kafka-27.png)

  - `acks=all`: Producer chờ xác nhận từ tất cả các ISR. Đảm bảo tính bền vững cao nhất vì dữ liệu chỉ được xem là gửi thành công khi đã sao chép đến tất cả các replica trong ISR.

  ![acks = all](/images/kafka-28.png)

- **min.insync.replicas**: Được cấu hình ở mức broker hoặc topic, giúp xác định số replica đồng bộ tối thiểu để chấp nhận ghi dữ liệu. Ví dụ, với `min.insync.replicas=2`, ít nhất hai replica trong ISR phải nhận dữ liệu để xác nhận thành công.

### Kafka Topic Durability & Availability

- Với replication factor là N, Kafka có thể chịu được đến N-1 broker gặp sự cố mà vẫn đảm bảo dữ liệu.
- Khả năng đọc:
  - Chỉ cần có một ISR, topic vẫn sẵn sàng cho các yêu cầu đọc.
- Khả năng ghi:
  - Với `acks=0` và `acks=1`: Cần có một ISR là đủ.
  - Với `acks=all` và `min.insync.replicas=M`: Có thể chịu tối đa N-M broker gặp sự cố.

### Kafka Consumers: Replicas Fetching

- Mặc định, các Kafka consumers sẽ đọc dữ liệu từ partition leader. Tuy nhiên, từ Kafka phiên bản 2.4, consumers có thể được cấu hình để đọc từ các ISR gần nhất nhằm giảm độ trễ và chi phí mạng.

  ![Consumers Replicas Fetching](/images/kafka-30.png)

### Preferred Leader & Leader Election

- **Preferred Leader** là leader mặc định của partition khi tạo topic. Khi leader này bị lỗi, một ISR sẽ được chọn làm leader tạm thời. Sau khi preferred leader khôi phục và dữ liệu đồng bộ, nó sẽ tiếp tục vai trò leader của mình.

Nhờ vào cơ chế sao chép này, Kafka có khả năng đảm bảo độ tin cậy cao cho dữ liệu, ngay cả khi có các sự cố xảy ra trong môi trường sản xuất.
