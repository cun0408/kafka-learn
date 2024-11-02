# Kafka Producers

Khi một topic đã được tạo trong Kafka, bước tiếp theo là gửi dữ liệu vào topic đó. Đây là lúc Kafka Producers phát huy vai trò của mình.

## Kafka Producers

Các ứng dụng gửi dữ liệu vào các topic được gọi là Kafka producers. Các ứng dụng thường tích hợp một client library Kafka để ghi dữ liệu vào Apache Kafka. Có nhiều client library xuất sắc cho hầu hết các ngôn ngữ lập trình phổ biến ngày nay, bao gồm Python, Java, Go và nhiều ngôn ngữ khác.

![Kafka Producer](/images/kafka-07.png)

Các Kafka Producers gửi dữ liệu vào Kafka. Những messages này sau đó được chuyển đến các topic và partition tương ứng bởi broker. Một producer gửi messages đến một topic, và các messages này được phân phối đến các partition theo một cơ chế như băm key (key hashing).

Để một messages được ghi thành công vào một topic Kafka, producer phải chỉ định một mức độ xác nhận (acks). chủ đề này sẽ được giới thiệu chi tiết hơn trong phần về sao chép (replication).

## Message Keys

Mỗi message event trong Kafka bao gồm một key tùy chọn và một giá trị (value).

Không có key: Nếu producer không chỉ định key (key=null), các message sẽ được phân phối đồng đều giữa các partition trong một topic. Điều này có nghĩa là các message sẽ được gửi theo cách vòng tròn (round-robin), tức là message đầu tiên đến partition p0, sau đó là p1, p2, và tiếp tục trở lại p0, và cứ như vậy.

Có key: Nếu một key được gửi (key != null), tất cả các message chia sẻ cùng một key sẽ luôn được gửi và lưu trữ trong cùng một partition Kafka. key có thể là bất kỳ thứ gì dùng để xác định một message - có thể là chuỗi, giá trị số, giá trị nhị phân, v.v.

Ứng dụng của key trong Kafka
key của message trong Kafka thường được sử dụng khi cần đảm bảo thứ tự message cho tất cả các message chia sẻ cùng một trường. Ví dụ, trong kịch bản theo dõi xe tải trong một đội xe, chúng ta muốn dữ liệu từ các xe tải được sắp xếp theo thứ tự ở mức độ từng xe tải. Trong trường hợp này, chúng ta có thể chọn key là truck_id.

Ví dụ trong trường hợp này, dữ liệu từ xe tải có id là truck_id_123 sẽ luôn được gửi đến partition p0. Điều này đảm bảo rằng tất cả các event liên quan đến xe tải cụ thể này sẽ được xử lý theo thứ tự mà chúng được gửi, giúp duy trì tính nhất quán trong việc theo dõi và xử lý dữ liệu.

![Fleet Example](/images/kafka-08.png)

## Cấu trúc của một Kafka Message

Một message Kafka được tạo bởi producer, bao gồm các thành phần chính sau:

![Structure of a Kafka Message](/images/kafka-09.png)

**Key:**
Là key tùy chọn trong message Kafka và có thể để trống (null).
Key có thể là một chuỗi, số, hoặc bất kỳ đối tượng nào, và được chuyển thành dạng nhị phân thông qua quá trình tuần tự hóa (serialization).
Nếu có key, các message có cùng key sẽ luôn đi vào cùng một partition, đảm bảo thứ tự xử lý cho những message đó.

**Value:**
Giá trị thể hiện nội dung chính của message và cũng có thể là null.
Giá trị có thể ở bất kỳ định dạng nào và sẽ được chuyển thành nhị phân trước khi gửi.

**Compression Type:**
Các message Kafka có thể được nén. Loại nén có thể được chỉ định là một phần của message.
Các tùy chọn nén bao gồm: none (không nén), gzip, lz4, snappy, và zstd.

**Headers:**
Kafka cho phép thêm các tiêu đề tùy chọn vào message dưới dạng các cặp key-value.
Headers thường được sử dụng để thêm thông tin metadata về message, đặc biệt hữu ích cho việc theo dõi và ghi log.

**Partition + Offset:**
Khi một message được gửi vào một topic Kafka, nó sẽ nhận một số partition và một ID offset.
Sự kết hợp của topic + partition + offset sẽ xác định duy nhất một message.

**Timestamp:**
Mỗi message sẽ có một timestamp, được thêm bởi người dùng hoặc hệ thống.
Timestamp giúp ghi lại thời điểm gửi hoặc xử lý message, hữu ích cho việc phân tích và quản lý dữ liệu theo thời gian.
Cấu trúc này cho phép Kafka quản lý và tổ chức message một cách hiệu quả, đồng thời đảm bảo tính linh hoạt và khả năng mở rộng trong việc xử lý dữ liệu.

## Kafka Message Serializers

Trong nhiều ngôn ngữ lập trình, các key và value của message thường được thể hiện dưới dạng đối tượng (object) để dễ đọc mã lệnh. Tuy nhiên, Kafka brokers yêu cầu các key và value của message ở dạng mảng byte. Quá trình chuyển đổi đối tượng từ ứng dụng producer thành dạng nhị phân được gọi là message serialization.

![Message Serialization](/images/kafka-10.png)

Ví dụ, nếu chúng ta có một message với:

Key là số nguyên (Integer)
Value là chuỗi (String)
Chúng ta cần sử dụng:

IntegerSerializer để chuyển đổi Integer thành mảng byte
StringSerializer để chuyển đổi String thành mảng byte

Các Serializer có sẵn trong Java Client SDK của Apache Kafka:
StringSerializer: Dùng cho chuỗi văn bản, cũng có thể thay thế JSON.
IntegerSerializer: Dùng cho số nguyên.
FloatSerializer: Dùng cho số thực.

Các Serializer phổ biến khác:
Bên cạnh các serializer có sẵn, còn có các bộ serializer khác có thể được viết riêng hoặc tải về để sử dụng với các định dạng như JSON-Schema, Apache Avro, và Protobuf. Các serializer này thường được tích hợp và hỗ trợ bởi Confluent Schema Registry để quản lý schema một cách hiệu quả và nhất quán khi xử lý dữ liệu.

## Kafka Message Key Hashing

Một partitioner trong Kafka là logic mã hóa có nhiệm vụ xác định xem một message sẽ được gửi vào partition nào.

Kafka Producers sử dụng một cơ chế partition mặc định để gán message vào partition thích hợp của topic Kafka. Nếu một message có key, cơ chế này sẽ sử dụng key đó để xác định partition, đảm bảo rằng tất cả các message có cùng key sẽ đi vào cùng một partition.

![Message Key Hashing](/images/kafka-11.png)

Hashing key là quá trình ánh xạ một key đến một partition cụ thể.

Kafka sử dụng thuật toán **murmur2** để băm key. Công thức tính toán partition mục tiêu (targetPartition) là:

`targetPartition = Math.abs(Utils.murmur2(keyBytes)) % (numPartitions - 1)`

Điều này đảm bảo rằng các message có cùng key sẽ luôn được gửi đến cùng một partition.

**Ghi đè Partitioners Mặc Định**
Người dùng có thể ghi đè cơ chế partition mặc định của Kafka bằng thuộc tính partitioner.class trong producer. Tuy nhiên, việc thay đổi partitioner không được khuyến nghị trừ khi bạn thực sự hiểu rõ về cách partition và yêu cầu cụ thể của hệ thống, vì điều này có thể ảnh hưởng đến cách dữ liệu được phân phối và sắp xếp trong các partition.
