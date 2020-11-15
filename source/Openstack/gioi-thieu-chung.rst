Giới thiệu chung
==================


Tổng quan về Cloud computing
--------------------------------

Cloud computing là sự tổng hòa các khái niệm như Web service, Web 2.0 và các khái niệm mới khác cũng như các xu hướng công nghệ nổi bật, dựa trên nền tảng Internet nhằm đáp ứng nhu cầu của người sử dụng. Ví dụ, dịch vụ Google Application Engine hay Amazon EC2 cung cấp những ứng dụng liên quan đến mua bán trực tuyến, được truy nhập từ một trình duyệt web, còn các phần mềm và dữ liệu đều được lưu trữ trên các server hay các datacenter.

Cloud computing còn được định nghĩa là mô hình cung cấp các tài nguyên hệ thống máy tính (như network, server, storage, ứng dụng và dịch vụ), đặc biệt là khả năng lưu trữ và khả năng tự động xử lý mà người dùng không quản trị một cách trực tiếp. Cloud computing còn được mô tả việc nhiều người dùng sử dụng tài nguyên của các data center thông qua Internet. Các hệ thống Cloud computing thường phân tán các tính năng tại các vị trí khác nhau trong các cụm server.

.. figure:: https://user-images.githubusercontent.com/65669673/98472476-e79ea400-2225-11eb-9c49-e49b4f0b190e.png

Tổng quan về Private Cloud
---------------------------

Private Cloud được định nghĩa là các dịch vụ được cung cấp qua Internet hoặc mạng nội bộ và bị giới hạn người dùng thay vì cho phép truy cập công khai do vậy còn được gọi là Internal Cloud hay Corporate Cloud. Private Cloud hỗ trợ doanh nghiệp những tiện ích như self service, khả năng mở rộng và tính linh hoạt như khi sử dụng Public Cloud. Ngoài ra, Private Cloud còn cung cấp tính riêng tư và độ bảo mật cấp độ cao thông qua các firewall và internal hosting để đảm bảo các hoạt động và dữ liệu quan trọng không thể truy cập bởi nhà cung cấp dịch vụ bên thứ ba.

Hai mô hình dịch vụ được áp dụng trong Private Cloud bao gồm: Platform as aService (PaaS) và Infrastructure as a Service (IaaS). Mô hình đầu tiên là Platform as a Service (PaaS) cho phép một tổ chức cung cấp mọi thứ từ các ứng dụng miễn phí cho đến các ứng dụng trả phí. Mô hình thứ hai là Infrastructure as a Service (IaaS) cho phép một tổ chức sử dụng tài nguyên của cơ sở hạ tầng như máy tính, hệ thống mạng và các thiết bị lưu trữ như một dịch vụ.

Private Cloud còn kết hợp với Public Cloud để tạo ra Hybrid Cloud cho phép doanh nghiệp tận dụng Cloud Bursting để tối ưu không gian và quy mô các dịch vụ Cloud Computing khi người dùng hay tổ chức tăng nhu cầu sử dụng.

Openstack là một trong những nền tảng phần mềm điển hình cho mô hình Private Cloud.


Tổng quan về Virtualization
----------------------------

Ảo hóa (virtualization) được định nghĩa là sự triển khai một hệ thống máy tính ảo trên nền một hệ thống máy tính thật. Ngoài ra, ảo hóa đề cập đến việc giả lập mọi thiết bị bằng bao gồm sự ảo hóa các nền tảng phần cứng máy tính, các thiết bị lưu trữ, tài nguyên cũng như hệ thống mạng máy tính. Nói cách khác, ảo hóa cũng có thể được coi là một kỹ thuật cho phép người dùng chia sẻ một instance vật lý của một tài nguyên hoặc một ứng dụng giữa nhiều người dùng và tổ chức khác nhau.

Ý tưởng của ảo hóa không phải là điều gì mới. Ý tưởng này được IBM giới thiệu vào năm 1960 khi các mainframe được sử dụng. Các mainframe hầu như không được sử dụng hết hiệu suất cũng như tính năng. Ảo hóa là một phương pháp tối ưu trong việc cung cấp các tài nguyên hệ thống cho các ứng dụng khác nhau hoạt động trên các mainframe.

Do sự phát triển của công nghệ như Utility Computing và Cloud Computing, công nghệ ảo hóa được chú trọng hơn trong sự phát triển của các công nghệ mới gần đây.

Nền tảng hypervisor được giới thiệu sau đây cũng sử dụng công nghệ ảo hóa.

Tổng quan về Hypervisor
----------------------------

Theo Redhat, hypervisor là phần mềm khái quát phần cứng từ một hệ điều hành
cho phép nhiều hệ điều hành cùng chạy trên cùng nền tảng phần cứng. Hypervisor
chạy trên hệ thống cho phép các máy ảo chạy trên nền phần cứng của host.

Theo VMWare, hypervisor là phần mềm cung cấp các tính năng phân vùng ảo
chạy trực tiếp trên phần cứng, nhưng ảo hóa các dịch vụ mạng ở mức tối đa.

Hypervisor là phần mềm máy tính, firmware hay phần cứng nhằm tạo và chạy
máy ảo. Một máy tính mà một hypervisor chạy một hay nhiều máy ảo được gọi là máy
Host và mỗi máy ảo được gọi là máy Guest.

Hypervisor biểu diễn bằng một nền tảng vận hành ảo chứa Guest OS và quản lý
vận hành Guest OS. Các instance trong các hệ điều hành có thể chia sẻ nhau các tài
nguyên phần cứng ảo, chẳng hạn, các instance chứa Linux, Windows và macOS có thể
cùng chạy trên một máy tính đơn x86. Cơ chế Hypervisor trái ngược với OS
virtualization, tại đó tất cả các instance (container) phải chia sẻ cùng một nhân (kernel)
thông qua Guest OS để phân chia các không gian sử dụng khác nhau.

Do sự phát triển của công nghệ ảo hóa nên các nền tảng phần cứng cũng có sự
thay đổi. Intel hay AMD đã thiết kế các hệ thống vi xử lý mới mở rộng từ kiến trúc
x86 tương ứng với những công nghệ được biết ngày nay là Intel VT-x hay AMD-V.
Chipset Intel 80286 đã được giới thiệu về 2 phương thức về địa chỉ bộ nhớ: địa chỉ bộ
nhớ thực (real mode) và địa chỉ bộ nhớ ảo (protected mode). Địa chỉ bộ nhớ ảo cung
cấp các tính năng hỗ trợ multicasting như phần cứng hỗ trợ bộ nhớ ảo và thành phần vi
xử lý.

Dựa trên nền tảng đó, Hypervisor được phân thành 2 loại như sau: Native
hypervisor (Bare-metal hypervisor), Hosted hypervisor.
