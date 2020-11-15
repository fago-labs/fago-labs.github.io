Cài đặt Openstack
========================

Một trong những cách cài OpenStack trên máy chủ Ubuntu đơn giản nhất là
thông qua DevStack. DevStack là một loạt các script được sử dụng để mang lại môi
trường OpenStack hoàn chỉnh dựa trên phiên bản mới nhất.

Mặc dù là giải pháp đơn giản nhất, tuy nhiên việc cài OpenStack trên máy chủ
Ubuntu bằng DevStack cũng mất khá nhiều thời gian, có thể từ 30-60 phút.

Chuẩn bị
--------------------------------

- Phần mềm VMWare
- CPU: 4vCPU
- RAM: 8GB
- DISK: 30GB
- 2 Network Adapter. Trong đó:

    + 1 Network Adapter NAT dùng để đi ra ngoài internet
    + 1 Network Adapter Private dùng để host OpenStack Image Ubuntu 64-bit server 18.04

Sau khi cài đặt xong máy ảo Ubuntu 18.04, bật tất cả network interface.


Cài đặt
---------------

Bước đầu tiên cần làm để cài OpenStack trên máy chủ Ubuntu bằng DevStack
là tạo một tài khoản người dùng không root, sau đó sử dụng tài khoản này để cài
OpenStack. Mở cửa sổ Terminal, sau đó nhập lệnh dưới đây vào:

::

    sudo useradd -s /bin/bash -d /opt/stack -m stack

Vì người dùng này sẽ thực hiện nhiều thay đổi đối với hệ thống của bạn, nên
người dùng sẽ có các đặc quyền sudo:

::

    echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack

Thay đổi sang người dùng vừa tạo bằng lệnh:

::

    sudo su - stack

Download DevStack với ``--branch stable/(version)`` để chọn phiên bản:

::

    git clone https://git.openstack.org/openstack-dev/devstack --branch stable/train

Sau khi đã download xong thì di chuyển vào folder devstack, tạo file ``local.conf``
bằng các lệnh:

::

    cd devstack/
    nano local.conf

Trước khi thực thi lệnh cài đặt, sẽ phải cấu hình file local.conf. Mở file
local.conf bằng cách sử dụng lệnh ``nano local.conf``. Sau đó điền nội dung như sau:

::

    [[local|localrc]]
    HOST_IP=SERVER_IP
    ADMIN_PASSWORD=PASSWORD
    SERVICE_PASSWORD=$ADMIN_PASSWORD
    DATABASE_PASSWORD=$ADMIN_PASSWORD
    RABBIT_PASSWORD=$ADMIN_PASSWORD

*Lưu ý:* Trong đoạn mã trên, thay thế PASSWORD bằng một mật khẩu duy nhất
mà bạn muốn sử dụng và SERVER_IP bằng địa chỉ IP của máy chủ OpenStack. Sau
khi hoàn tất lưu và đóng file lại.

Bước tiếp theo bây giờ là thực thi lệnh để cài đặt OpenStack. Sử dụng lệnh sau
để cài đặt:

::

    ./stack.sh

Quá trình cài đặt sẽ mất khoảng 30-60 phút để hoàn tất.
