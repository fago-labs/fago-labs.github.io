Vận hành Openstack trên Dashboard
===================================

OpenStack Dashboard là một giao diện dựa trên web cho phép bạn quản lý các
tài nguyên và dịch vụ của OpenStack. Bảng điều khiển cho phép bạn tương tác với bộ
điều khiển đám mây OpenStack Compute bằng cách sử dụng các API OpenStack.

Để truy cập vào OpenStack Dashboard thì mở trình duyệt lên, truy cập vào địa
chỉ host của OpenStack và đăng nhập với username: admin và password đã cài đặt
trong file local.conf.

.. figure:: https://user-images.githubusercontent.com/65669673/98481954-abbd0c00-2230-11eb-9270-b0fb7814526c.png

Network Management
--------------------------

Dịch vụ Mạng OpenStack cung cấp một hệ thống có thể mở rộng để quản lý kết
nối mạng trong triển khai đám mây OpenStack. Nó có thể dễ dàng và nhanh chóng
phản ứng với các nhu cầu mạng thay đổi.

Tạo Provider Network
^^^^^^^^^^^^^^^^^^^^^

Để tạo provider network truy cập vào Admin > Network > Networks > Create Network

.. figure:: https://user-images.githubusercontent.com/65669673/98482058-4b7a9a00-2231-11eb-9ad5-7993eab957a9.png

Sau đó thiết lập thông tin cho mạng

.. figure:: https://user-images.githubusercontent.com/65669673/98482088-7664ee00-2231-11eb-9f52-a8fbc2205c37.png

Tiếp đến tạo subnet cho mạng

.. figure:: https://user-images.githubusercontent.com/65669673/98482118-aad8aa00-2231-11eb-8b11-ad774ee9c761.png

Cuối cùng cấu hình chi tiết cho mạng

.. figure:: https://user-images.githubusercontent.com/65669673/98482129-c2b02e00-2231-11eb-9c05-7ed93c2e3a02.png

Tạo Tenant Network
^^^^^^^^^^^^^^^^^^^

Để tạo tenant network thì cũng tương tự như tạo provider network chỉ thay đổi
tên thông tin mạng và subnet

.. figure:: https://user-images.githubusercontent.com/65669673/98482147-f8551700-2231-11eb-8257-3ea765e3b3d6.png

Tạo Tenant Router
^^^^^^^^^^^^^^^^^^

Để tạo tenant router truy cập vào Project > Network > Routers > Create Router

.. figure:: https://user-images.githubusercontent.com/65669673/98482178-4538ed80-2232-11eb-8311-da0e34ae7df5.png

Sau đó thiết lập thông tin cho router

.. figure:: https://user-images.githubusercontent.com/65669673/98482449-11f75e00-2234-11eb-8c2d-96ee2b048192.png

Để thêm interface của tenant network vào router vừa tạo, chọn router “pro-ten”, chuyển qua tab Interfaces, chọn Add Interface

.. figure:: https://user-images.githubusercontent.com/65669673/98482463-305d5980-2234-11eb-8e3a-820b8cfde2d4.png

Tạo Floating IP để truy cập instance từ mạng ngoài
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Để gán floating ip cho một instance truy cập vào Project > Instances > Associate Floating IP

.. figure:: https://user-images.githubusercontent.com/65669673/98482464-318e8680-2234-11eb-93f3-318146946d72.png

Chọn biểu tượng dấu "+" để tạo mới floating ip

.. figure:: https://user-images.githubusercontent.com/65669673/98482465-32bfb380-2234-11eb-8cc5-53464116bbe9.png

Chọn provider-net, sau đó chọn Allocate IP

.. figure:: https://user-images.githubusercontent.com/65669673/98482467-33f0e080-2234-11eb-8943-05cfdf31cbeb.png

Sau đó chọn Associate

.. figure:: https://user-images.githubusercontent.com/65669673/98482468-35220d80-2234-11eb-989a-a9c9b86e9411.png

Instance Management
---------------------

Là người dùng quản trị, có thể quản lý các instance cho người dùng trong các
dự án khác nhau. Có thể xem, chỉnh sửa, thực hiện khởi động lại, tạo snapshot và di
chuyển các instance.


Tạo flavor, cloud images, keypair
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Để tạo flavor, vào Admin > Compute > Flavors > Create Flavor

.. figure:: https://user-images.githubusercontent.com/65669673/98482470-36533a80-2234-11eb-8969-c930de7ecddb.png

Để tạo Image, vào Admin > Compute > Images > Create Image và thiết lập như sau (sử dụng cloud image như trên trang chủ khuyến cáo)

.. figure:: https://user-images.githubusercontent.com/65669673/98482472-37846780-2234-11eb-90dd-44395058b696.png

Để tạo keypair vào Project > Compute > Key Pairs > Create Key Pair. Sau khi tạo xong ssh key, trình duyệt sẽ tự động tải về file key dùng để ssh.

.. figure:: https://user-images.githubusercontent.com/65669673/98482473-394e2b00-2234-11eb-975e-ef80c50612a3.png

Tạo Security Group
^^^^^^^^^^^^^^^^^^^

Để tạo Security Group vào Project > Network > Security Groups > Create Security Group

.. figure:: https://user-images.githubusercontent.com/65669673/98482577-f476c400-2234-11eb-831d-478ccd000b23.png

Sau đó cần thêm các rule vào Security Group mới tạo. Vào Manage Rules của “test”, rồi chọn Add Rule

Thêm rule All ICMP để có thể ping instance

.. figure:: https://user-images.githubusercontent.com/65669673/98482579-f5a7f100-2234-11eb-82a9-03d2c9134cd6.png

Thêm rule SSH để có thể ssh instance

.. figure:: https://user-images.githubusercontent.com/65669673/98482581-f771b480-2234-11eb-9af7-41ebb3e8c189.png

Tạo instance
^^^^^^^^^^^^^

Để tạo Instance, vào Project > Compute > Instances > Launch Instance. Sau đó cài đặt các thông số cho instance

.. figure:: https://user-images.githubusercontent.com/65669673/98482582-f8a2e180-2234-11eb-8fdc-7bae99f7e1fa.png

Chọn Source

.. figure:: https://user-images.githubusercontent.com/65669673/98482584-f9d40e80-2234-11eb-8d9e-63b244d77eb9.png

Chọn flavor

.. figure:: https://user-images.githubusercontent.com/65669673/98482585-fa6ca500-2234-11eb-80a7-400ebba9649a.png

Chọn network

.. figure:: https://user-images.githubusercontent.com/65669673/98482587-fcceff00-2234-11eb-972b-7a423b60b86e.png

Chọn Security Groups

.. figure:: https://user-images.githubusercontent.com/65669673/98482588-fe002c00-2234-11eb-81f9-9d615961c0d4.png

Chọn Key Pair

.. figure:: https://user-images.githubusercontent.com/65669673/98482589-ff315900-2234-11eb-94eb-dc02ed5a1374.png

Sau khi đã thiết lập xong thông số cho instance thì chọn Launch Instance để khởi tạo instance

User, Project Management
-------------------------

Quản trị viên OpenStack có thể tạo dự án và tạo tài khoản cho người dùng mới
bằng OpenStack Dashboard. Dự án sở hữu các tài nguyên cụ thể trong môi trường
OpenStack của bạn. Bạn có thể liên kết người dùng với các vai trò, dự án hoặc cả hai.

Quản lý user
^^^^^^^^^^^^^^

Để quản lý 1 người dùng ta vào Identity > Users

- Để thêm 1 người dùng vào Create user
- Để chỉnh sửa 1 người dùng vào Edit (chọn vào phần người dùng muốn sửa)
- Để đổi mật khẩu người dùng chọn Change Password trên thanh hành động của người dùng đó
- Để xóa 1 người dùng thì chọn người dùng muốn xóa rồi ấn Delete Users

.. figure:: https://user-images.githubusercontent.com/65669673/98482592-00628600-2235-11eb-8dcb-d2ae942cff3f.png

Quản lý project
^^^^^^^^^^^^^^^^

Để quản lý project ta vào Identity > Projects

- Để thêm 1 project vào Create Project
- Để chỉnh sửa project chọn thanh hành động của project muốn sửa và chọn tùy chọn cần sửa
- Để xóa 1 project thì chọn project muốn xóa rồi ấn Delete Projects

.. figure:: https://user-images.githubusercontent.com/65669673/98482593-0193b300-2235-11eb-9ae6-b73caace01d0.png

Quota cho các project
^^^^^^^^^^^^^^^^^^^^^^^^

Để ngăn dung lượng hệ thống bị cạn kiệt mà không có thông báo, bạn có thể
thiết lập các quotas - là giới hạn hoạt động. Ví dụ: có thể kiểm soát số lượng gigabyte
cho phép cho mỗi dự án để tài nguyên đám mây được tối ưu hóa. Quota có thể được
thực thi ở cả cấp độ dự án và người dùng dự án.

Sử dụng Dashboard, bạn có thể xem quota của Compute và Block Storage mặc định
cho các dự án mới, cũng như cập nhật quota cho các dự án hiện có.

Để xem default project quotas vào Admin > System > Defaults

.. figure:: https://user-images.githubusercontent.com/65669673/98482594-02c4e000-2235-11eb-883e-940d023c9675.png

Để Update project quotas vào Admin > System > Defaults > Update Defaults

.. figure:: https://user-images.githubusercontent.com/65669673/98482597-048ea380-2235-11eb-9663-39d30f9f1211.png

