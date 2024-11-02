# Kafka Consumers

Kafka Consumers là các ứng dụng đọc dữ liệu từ các topic trong Apache Kafka. Khi dữ liệu đã được đặt vào một topic, các ứng dụng có thể tiêu thụ dữ liệu đó bằng cách sử dụng các thư viện client Kafka phổ biến, có sẵn cho nhiều ngôn ngữ như Python, Java, Go, và nhiều ngôn ngữ khác.

![Kafka Consumers](/images/kafka-12.png)

- **Đọc dữ liệu tuần tự**: Consumers đọc dữ liệu theo thứ tự trong mỗi partition và không thể đọc ngược lại (do cách Kafka và client được triển khai).
- **Không đảm bảo thứ tự toàn bộ**: Nếu consumer đọc từ nhiều partition cùng lúc, thứ tự thông điệp sẽ không được đảm bảo giữa các partition, nhưng vẫn giữ được thứ tự trong từng partition riêng lẻ.
- **Mô hình Pull**: Consumers yêu cầu dữ liệu từ các brokers theo yêu cầu, giúp điều chỉnh tốc độ tiêu thụ phù hợp với nhu cầu của ứng dụng.

## Message Deserializers

Khi dữ liệu được gửi vào Kafka, Kafka producer sẽ **serialize** dữ liệu thành byte để lưu trữ trong các topic. Ngược lại, khi Kafka consumer đọc dữ liệu từ Kafka, nó phải **deserialize** dữ liệu về định dạng ban đầu để ứng dụng có thể sử dụng.

- **Tuân thủ định dạng**: Dữ liệu phải được deserialized cùng định dạng mà nó được serialized trước đó. Ví dụ:
  - Nếu producer dùng `StringSerializer`, consumer phải dùng `StringDeserializer`.
  - Nếu producer dùng `IntegerSerializer`, consumer phải dùng `IntegerDeserializer`.

![Deserializers](/images/kafka-13.png)

## Poison Pills

Nếu dữ liệu trong một Kafka topic không tuân thủ định dạng đã thống nhất, dữ liệu đó được gọi là **poison pills**. Các poison pills có thể khiến ứng dụng gặp lỗi hoặc dữ liệu không nhất quán, gây ra khó khăn trong quá trình xử lý và gỡ lỗi.

## Thay đổi Định dạng Dữ liệu

Kafka khuyến nghị rằng **định dạng của topic không nên thay đổi trong suốt vòng đời của topic**. Nếu muốn chuyển đổi định dạng dữ liệu (ví dụ từ JSON sang Avro), bạn nên tạo một topic mới và chuyển ứng dụng của mình để sử dụng topic mới đó.
