# Tổng quan
Thư viên pcl_features chưa cấu trúc dữ liệu và cơ chế cho các chức năng 3D từ đấm mây điểm. Các tính năng 3D là đại diện tại một điểm hoặc ví trí 3D nhất định trong không gian, mô tả mô hình hình học dựa trên thông tin có sẳn xung quanh điểm. Không gian dữ liệu được chọn xung quanh điểm truy vấn thường gọi là **k-neighborhood**. <br/>

Hình dưới đây cho thấy một ví dụ đơn gian của một điểm truy vấn được chọn, và nó chọn k-neighborhood.<br/>
<br/>
![ScreenShot](https://github.com/bigkizd/Point-Cloud-Library/blob/master/Modules/features/image/features_normal.png)
<br/>

Một ví dụ về hai trong số các tính năng hình học điểm được sử dụng rộng rãi là *the underlying surface's estimated curvature and normal at a query point **p***. Cả hai được coi là các tính năng cục bộ, như họ mô tả một điểm bằng cách sử dụng thông tin cung cấp bởi các điểm lân cận của nó. Xác định hàng xóm hiểu quả, bộ dữ liệu đầu vào thường được chia thành nhiều phần nhỏ hơn bằng cách sử dụng các kỷ thuật phân chia không gian như octrees or kD-trees(xem hình dưới-trái:kD-tree, right: octree).
