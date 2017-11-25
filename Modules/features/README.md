# Tổng quan
Thư viên pcl_features chưa cấu trúc dữ liệu và cơ chế cho các chức năng 3D từ đấm mây điểm. Các tính năng 3D là đại diện tại một điểm hoặc ví trí 3D nhất định trong không gian, mô tả mô hình hình học dựa trên thông tin có sẳn xung quanh điểm. Không gian dữ liệu được chọn xung quanh điểm truy vấn thường gọi là **k-neighborhood**. <br/>

Hình dưới đây cho thấy một ví dụ đơn gian của một điểm truy vấn được chọn, và nó chọn k-neighborhood.<br/>
<br/>
![ScreenShot](https://github.com/bigkizd/Point-Cloud-Library/blob/master/Modules/features/image/features_normal.png)
<br/>

Một ví dụ về hai trong số các tính năng hình học điểm được sử dụng rộng rãi là *the underlying surface's estimated curvature and normal at a query point **p***. Cả hai được coi là các tính năng cục bộ, như họ mô tả một điểm bằng cách sử dụng thông tin cung cấp bởi các điểm lân cận của nó. Xác định hàng xóm hiểu quả, bộ dữ liệu đầu vào thường được chia thành nhiều phần nhỏ hơn bằng cách sử dụng các kỷ thuật phân chia không gian như octrees or kD-trees(xem hình dưới-trái:kD-tree, right: octree) và sau đố tìm kiếm điểm gần nhất được tối ưu trong không gian bộ nhớ. Tùy thuộc vào ứng dụng có thể lựa chọn để xác định một số cố định của k điểm trong vùng lân cận của p, hoặc tất cả các điểm được tìm thấy bên trong hình cấu bán kính r và tâm là p. Không thể phũ nhận, một trong phương thức dễ dàng nhất để ước tính các tiểu chuẩn bề mặt và thay đổi độ cong tại điểm p là thực hiện phân tích eigen(Ví dụ: tính toán vector eigen và giá trị eigen) của k-neighborhood điểm bề mặt. Như vậy, vector riêng tương ứng với giá trị riêng nhỏ nhất sẽ gần đúng bề mặt  n tại điểm p, trong khi sự thảy đổi sẽ được ước lượng từ các giá trị riêng như:
       
![toan](https://github.com/bigkizd/Point-Cloud-Library/blob/master/Modules/features/image/features_bunny.png)


# Features TUTORIAL
## How 3D Features work in PCL
PCL features thường sử dụng các phương thức gần đúng để tính hàng xóm gần nhất của điêm truy vấn, sừ dụng truy vân nhanh như cậu trúc dự liệu kd-tree.  Có hai loại truy vấn được quan tâm là: <br/>
   * xác định **k**(người dùng cho tham số) hàng xóm của một truy vấn điểm( cúng được hiểu như *k-search*);
   * xác định **tất cả hàng xóm** của truy vấn điểm trong hình cầu bán kính r(cúng được hiểu như *radius-search*).
## Estimating Surface Normals in a PointCloud

## Moment of inertia and eccentricity based descriptors
Convariance matrix của đám mây điểm được tính và giá trị và vector của nó được trích xuất. Bạn có thể xem xét vector eigen kết quả được chuẩn hóa luôn tạo thành hệ tọa độ thuận tay phải(vector eigen chính đại điện cho trục X và vector minor đại dienjn cho trục Z). Bước tiếp theo, quá trình lặp lại xảy ra. Mỗi vòng lặp chính vector eigen được xoay. Trật tự quay luôn giống nhau và được thực hiện xung quanh các vector khác, điều này cung caaos sự bất biến của phép quay của đám mây điểm. Từ đó chúng ta đề cập đến vector lớn quay như trụ hiện tại.
![toandaominh](https://github.com/bigkizd/Point-Cloud-Library/blob/master/Modules/features/image/eigen_vectors.png)

Đối với mỗi trục momen quán tính hiện tại được tính. Hơn nữa, Trục hiện tại được sử dụng để tính lệch tâm. Vì lý do này, vector hiện tại được coi là vector chuẩn của mặt phẳng và các đám mây đàu vào được chiều lên nó. Sau đó tính lệch tâm được tính cho phép chiều có được.

![dao](https://github.com/bigkizd/Point-Cloud-Library/blob/master/Modules/features/image/projected_cloud.png)
## RoPs (Rotational Projection Statistics) feature
Cho một mạng lưới và một tập các điểm cần làm các tính năng để thực hiện một số bước đơn giản. Trước tiên cho điểm trên bề mặt cục bộ là cắt. Bề mặt cục bố bao gồm các điểm và tam giác nằm trong bán kính hổ trợ đã cho. Cho bề mặt cục bộ LRF(*Local Reference Frame) được tính toán. LRF là bộ ba vector đơn giản, đầy đủ các thông tin về cách những vector này được tính toán, bạn sẽ tìm thấy trong mục này. Điều thực sự quan trọng là việc sử dụng các vector này chúng ta cung cấp sự bất biến để xoay các đám mây. Để làm điều đó, chúng tôi chỉ dịch các điểm của bề mặt địa phương theo cách mà điểm quan tâm trở thành gốc, sau đó chung ta xoay bề mặt theo cách mà điểm được quan tâm trở thành gốc, sau đó chúng ta sẽ quay bề mặt cục bộ cũng như LRF vector với trục Ox, Oy, và Oz. Sau khi thực hiện xong, chúng ta bắt đầu khai thác tính năng. Sau các bước thực hiện sau đối với mỗi trục Ox, Oy, và Oz, chúng ta sẽ đề cập đến các trục này như trục hiện tại: <br/>
   - Bề mặt cục bộ được xoay quanh trục hiện tại một góc nhất định
   - Điểm của xoay bề mặt cục bộ được chiếu lên XY, XA và YZ;
   - cho mỗi ma trận phân phối chiếu được xây dụng, ma trận này chỉ đơn giản cho thấy có bao nhiêu điểm rơi vào mỗi bun. Số bin đại diện cho kích thước ma trận và là tham số của thuật toán, cũng như hộ trở của bán kính.
   - mỗi ma trận phân phối khoảnh khắc trung tâm được tính toán: M11, M12, M21, M22, E. E là *Shannon entropy*;
   - Tính toán giá trị sau đó nối thánh các chức năng nhỏ.
