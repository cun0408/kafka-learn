# Kafka Topics

## Kafka Topic là gì ?

Tương tự như cách cơ sở dữ liệu sử dụng bảng để tổ chức và phân đoạn tập dữ liệu, Kafka sử dụng topic để tổ chức các message liên quan.

Mỗi topic được xác định bằng một tên cụ thể. Ví dụ, ta có thể có một topic tên là "logs" chứa các message nhật ký từ ứng dụng, và một topic khác tên là "purchases" chứa dữ liệu mua hàng từ ứng dụng khi giao dịch xảy ra.

![Topic in a Cluster](/images/kafka-04.png)

Kafka Topic có thể chứa bất kỳ loại message nào ở bất kỳ định dạng nào và trình tự của tất cả các message này được gọi là data stream.

Dữ liệu trong Kafka được xóa sau một tuần theo mặc định (còn được gọi là message retention period) và giá trị này có thể cấu hình được. Cơ chế xóa dữ liệu cũ nàyđảm bảo cụm Kafka không hết dung lượng đĩa bằng cách tái chế topic theo thời gian.

## Kafka Partition là gì ?

Các topic trong Kafka được chia thành nhiều partitions. Một topic có thể có nhiều partitions, và phổ biến là các topic có tới 100 partitions.

Số lượng partitions của một topic được xác định vào thời điểm tạo topic. Các partitions được đánh số từ 0 đến N-1, trong đó N là số lượng partitions. Mỗi partitions sẽ chứa các message được thêm vào cuối mỗi partitions.

![Topic Partition](/images/kafka-05.png)

Việc chia topic thành các partitions giúp cải thiện khả năng chịu lỗi. Mỗi message trong một partitions được gán một offset (giá trị nguyên) duy nhất, mà Kafka thêm vào khi message được ghi vào partitions.

Điều quan trọng là các topic trong Kafka là bất biến (immutable): một khi dữ liệu đã được ghi vào một partitions, nó không thể bị thay đổi.

## Ví dụ về Kafka Topic

![Fleet Tracking](/images/kafka-06.png)

Một công ty giao thông muốn theo dõi đội xe tải của mình. Mỗi xe tải được trang bị một thiết bị định vị GPS báo cáo vị trí của chúng cho Kafka. Chúng ta có thể tạo một topic có tên là - **trucks_gps** mà các xe tải sẽ công bố vị trí của chúng. Mỗi xe tải có thể gửi một message đến Kafka sau mỗi 20 giây, mỗi message sẽ chứa ID xe tải và vị trí xe tải (vĩ độ và kinh độ). topic có thể được chia thành một số partitions phù hợp, chẳng hạn như 10. Có thể có những người dùng khác nhau của topic. Ví dụ: một ứng dụng hiển thị vị trí xe tải trên bảng điều khiển hoặc một ứng dụng khác gửi thông báo nếu xảy ra sự kiện quan tâm.

## Kafka Offset là gì ?

Offsets trong Apache Kafka đại diện cho vị trí của một message trong một partitions của Kafka. Số hiệu offset cho mỗi partitions bắt đầu từ 0 và được tăng lên cho mỗi message gửi đến một partitions cụ thể. Điều này có nghĩa là các offset chỉ có ý nghĩa trong một partitions nhất định; ví dụ, offset 3 trong partitions 0 không đại diện cho cùng một dữ liệu như offset 3 trong partitions 1.

Nếu một topic có nhiều partitions, Kafka đảm bảo thứ tự của các message trong mỗi partitions, nhưng không có thứ tự đảm bảo giữa các partitions.

Mặc dù các message trong topic Kafka có thể bị xóa theo thời gian, các offset không được tái sử dụng. Chúng liên tục được tăng lên trong một chuỗi không bao giờ kết thúc.
