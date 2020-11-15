Giới thiệu về Openstack
========================


Tổng quan về Openstack
--------------------------------

Openstack là một nền tảng phần mềm mã nguồn mở Cloud Computing, được
phát triển theo mô hình Infrastructure as a Service (IaaS) quản lý tài nguyên hệ thống
máy tính và cung cấp tài nguyên (các server ảo và các tài nguyên khác) cho người
dùng. Nền tảng phần mềm bao gồm một nhóm các chức năng liên quan đến nhau điều
khiển xử lý các nhóm phần cứng, lưu trữ và hệ thống mạng trong data center. Người
sử dụng quản lý thông qua một dashboard dựa trên nền web, các công cụ dòng lệnh
hoặc thông qua các API RESTful.

Openstack.org là đơn vị phát hành Openstack dựa theo các điều khoản của Giấy
phép Apache.

OpenStack là một dự án chung của Rackspace Hosting và của NASA vào năm
2010. Kể từ năm 2016, OpenStack được quản lý bởi OpenStack Foundation (một tổ
chức phi lợi nhuận) được thành lập vào tháng 9 năm 2012 để quảng bá phần mềm
OpenStack và cộng đồng người dùng và hơn 500 công ty đã tham gia dự án.


Các module chính được cung cấp trong Openstack
------------------------------------------------

Openstack identity module
^^^^^^^^^^^^^^^^^^^^^^^^^^
OpenStack Identity (Keystone) cung cấp một danh mục các user được ánh xạ
tới các dịch vụ Openstack để người dùng có thể truy nhập. OpenStack Identity hoạt
động như một hệ thống xác thực chung trên toàn bộ hệ thống và có thể tích hợp với
các dịch vụ có danh mục phụ trợ hiện có như Lightweight Directory Access Protocol
(LDAP) hay Pluggable authentication module (PAM). OpenStack Identity hỗ trợ nhiều
hình thức xác thực bao gồm thông tin đăng nhập tên người dùng và mật khẩu tiêu
chuẩn, hệ thống token và phương thức truy nhập của AWS (tức là Amazon Web
Services). Ngoài ra, OpenStack Identity còn cung cấp một danh sách có thể truy vấn
của tất cả các dịch vụ được triển khai trên OpenStack trong một khởi tạo bình thường.
Người dùng và các công cụ của bên thứ ba có thể xác lập tài nguyên được cấp quyền
có thể truy cập thông qua OpenStack Identity.

Openstack compute module
^^^^^^^^^^^^^^^^^^^^^^^^^
OpenStack Compute (Nova) là một module điều khiển Cloud computing, là
phần chính của hệ thống Openstack được phát triển theo mô hình dịch vụ
Infrastructure as a Service (IaaS). OpenStack Compute được thiết kế để quản lý và tự
động tối ưu các tài nguyên máy tính cũng như có thể hoạt động với sự mở rộng các
công nghệ ảo hóa có sẵn bao gồm các cấu hình của bare-metal server hay cấu hình của siêu máy tính. KVM, VMware và Xen là những lựa chọn có sẵn sử dụng công nghệ
hypanneror (màn hình máy ảo), cùng với công nghệ Hyper-V và Linux container là
LXC.

Openstack network module
^^^^^^^^^^^^^^^^^^^^^^^^^
OpenStack Networking (Neutron) có chức năng quản lý mạng và địa chỉ IP.
OpenStack Networking đảm bảo mạng không bị tắc nghẽn hoặc thắt cổ chai trong khi
triển khai Cloud và cung cấp cho người dùng khả năng cấu hình nội bộ và cấu hình
qua mạng Internet.

OpenStack Networking cung cấp các mô hình mạng cho các ứng dụng hoặc
nhóm người dùng khác nhau. Các mô hình tiêu chuẩn bao gồm các flat network hoặc
VLAN để phân tách các server với nhau và lưu lượng truyền dẫn.

OpenStack Networking quản lý địa chỉ IP, hỗ trợ cả địa chỉ IP tĩnh hoặc địa chỉ
IP động. Địa chỉ Floating IP cho phép lưu lượng truy cập được định tuyến lại một cách
linh hoạt bất kỳ tài nguyên nào trong cơ sở hạ tầng, do đó người dùng có thể chuyển
hướng lưu lượng trong quá trình bảo trì hoặc trong trường hợp xảy ra lỗi.

Người dùng có thể tạo các mạng nội bộ, điều khiển lưu lượng, thiết lập kết nối
tới các server và các thiết bị trong một hoặc nhiều mạng. Quản trị viên có thể sử dụng
các công nghệ software-defined networking (SDN) như OpenFlow để hỗ trợ tối đa
multi-tenancy và triển khai quy mô rộng. OpenStack Networking cung cấp một
framework mở rộng có thể triển khai và quản lý các dịch vụ mạng thêm vào như hệ
thống phát hiện xâm nhập (IDS), cân bằng tải, tường lửa và mạng riêng ảo (VPN).

Openstack storage module
^^^^^^^^^^^^^^^^^^^^^^^^^
Openstack storage có 2 loại lưu trữ là Block storage (Cinder) và Object storage (Swift)

- OpenStack Block Storage (Cinder): là một hệ thống lưu trữ block-level để sử dụng với các OpenStack compute instance. Hệ thống block storage quản lý việc tạo, gắn và tách các khối thiết bị trên các server. Các phân vùng block storage được tích hợp hoàn toàn vào OpenStack Compute và Dashboard cho phép người dùng cloud quản lý lưu trữ cần thiết của người dùng. Ngoài lưu trữ trên server Linux cục bộ, block storage có thể sử dụng các nền tảng lưu trữ bao gồm Ceph, CloudByte, Coraid, …

- OpenStack Object Storage (Swift) là một hệ thống lưu trữ dự phòng có thể mở rộng. Các object và file được ghi trên nhiều ổ đĩa trải đều các server trong data center với phần mềm OpenStack chịu trách nhiệm đảm bảo sao chép và toàn vẹn dữ liệu thông qua cluster. Các cluster lưu trữ phân bố đều khi thêm các server mới. Nếu server hoặc ổ cứng bị lỗi, OpenStack sẽ sao chép nội dung của nó từ các node hoạt động khác sang các vị trí mới trong cluster. Vì OpenStack sử dụng tính logic trong phần mềm để đảm bảo sao chép và phân tán dữ liệu trên các thiết bị khác nhau nên ổ cứng và server được sử dụng không cần đắt tiền.

Openstack image module
^^^^^^^^^^^^^^^^^^^^^^^
OpenStack Image (Glance) cung cấp dịch vụ trải nghiệm, tạo lập và cho phép
sử dụng các image (ổ đĩa ảo). Các image lưu trữ được sử dụng như một template.
OpenStack Image cũng có thể được sử dụng để lưu trữ và lập danh mục không giới
hạn số lần sao lưu. Image Service có thể lưu trữ image trong nhiều loại back-end, bao
gồm Swift. Image Service API cung cấp giao diện REST tiêu chuẩn để truy vấn thông
tin về image ổ đĩa và cho phép các client tải các image sang server mới.

OpenStack Image thêm nhiều cải tiến cho cơ sở hạ tầng truyền thống. Nếu được
tích hợp với VMware, OpenStack Image giới thiệu các tính năng nâng cao cho tập các
vSphere như vMotion, tính sẵn sàng cao và lập lịch tài nguyên động (DRS). vMotion
là một công nghệ cho phép di chuyển trực tiếp một VM đang chạy, từ server vật lý này
sang server vật lý khác mà không bị gián đoạn dịch vụ. Do đó, OpenStack Image cho
phép một datacenter tự tối ưu việc tự động và điều phối, cho phép bảo trì phần cứng
cho các server hoạt động kém hiệu suất mà không bị gián đoạn.

Các module OpenStack khác cần tương tác với các image như Heat, phải giao
tiếp với images metadata thông qua Glance. Ngoài ra, Nova có thể tiếp nhận thông tin
về các image và sự thay đổi cấu hình trên image để tạo ra một instance. Tuy nhiên,
Glance là module duy nhất có thể thêm, xóa, chia sẻ hoặc sao chép image.

Openstack dashboard module
^^^^^^^^^^^^^^^^^^^^^^^^^^^
OpenStack Dashboard (Horizon) cung cấp cho quản trị viên và người dùng giao
diện đồ họa để truy cập, cung cấp và triển khai tự động các tài nguyên cloud-based.
Mô hình chứa các sản phẩm và dịch vụ của bên thứ ba như thanh toán, giám sát và các
công cụ quản lý bổ sung. OpenStack Dashboard cũng có khả năng tạo sự khác biệt
trong cách sử dụng cho các nhà cung cấp dịch vụ và các nhà cung cấp thương mại
khác. OpenStack Dashboard là một trong các cách người dùng có thể tương tác với tài
nguyên OpenStack. Các nhà phát triển có thể tự động truy cập hoặc xây dựng các công
cụ để quản lý tài nguyên bằng API OpenStack gốc hoặc API tương thích EC2.


Các thành phần chức năng chính của Openstack
---------------------------------------------

Dựa trên các dịch vụ chính, Openstack đưa ra mô tả chi tiết các thành phần chức năng như sau:

Controller node là một node dùng để cài đặt hầu hết các dịch vụ liên quan đến
quản trị, xác thực của Openstack cũng như các dịch vụ quản lý database cần thiết liên
quan đến các image và các máy ảo cho hay là một control plane trong môi trường
Openstack. Controller node chứa các module Keystone, Glance và Horizon.

Compute node là một node dùng đề cài đặt các dịch vụ quản lý các máy ảo.
Compute node chứa module Nova.

Network node là một node dùng để cài đặt các dịch vụ quản lý đến hệ thống
mạng và địa chỉ IP trong Openstack. Network node chứa module Neutron.

Storage node là một node dùng để cài đặt các dịch vụ liên quan đến quản lý lưu
trữ các image, các máy ảo cũng như các file trong Openstack. Storage node chứa
Cinder hoặc Swift hoặc cả Cinder lẫn Swift.
