OVS Architecture
================

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-44-19.png



OVS thường được sử dụng để kết nối nhiều VM/container trong một host. Nó quản lý cả cổng vật lý - physical port (ví dụ: eth0, eth1) và cổng ảo virtual port (tap devices of VMs).

Hình trên, OVS bao gồm ba thành phần:

**vswitchd**

- user space program, ovs deamon

- tools: ovs-appctl

**ovsdb-server**

- user space program, database server of OVS

- tools: ovs-vsctl, ovs-ofctl

**kernel module (datapath)**

- kernel space module, OVS packet forwarder

- tools: ovs-dpctl

vswitchd là main deamon process của OVS, ovsdb-server là database server của OVS và datapath là một kernel module thực hiện chuyển tiếp gói platform-dependent. Sau khi OVS khởi động, chúng ta có thể thấy hai dịch vụ: ovs-vswitchd và ovsdb-server:

ovs-ovswitchd nhận OpenFlow messages từ OpenFlow controller và OVSDB-protocol định dạng messages từ ovsdb-server. Giao tiếp giữa ovs-vswitchd và datapath thông qua netlink (một dòng socket tương tự như Unix Domain Socket).

Một hình minh họa khác chi tiết hơn:

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-44-36.png


1. vswitchd
------------

1.1. vswitchd Overview
--------------

ovs-vswitchd nằm ở vị trí quan trọng của OVS, cần tương tác với OpenFlow controller, OVSDB, và kernel module.

- ovs-vswitchd giao tiếp với:

- - outside world sử dụng OpenFlow

- - ovsdb-server sử dụng giao thức OVSDB protocol

- - kernel thông qua netlink (tương tự như Unix socket domain)

- - system thông qua abstract interface là netdev

- Triển khai mirroring, bonding, và VLANs

- Công cụ CLI: ovs-ofctl, ovs-appctl

vswitch module được chia thành nhiều submodules/libraies:

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-44-44.png


- ovs-vswitchd: vswitchd daemon

- ofproto: library định nghĩa (abstracts) ovs bridge

- ofproto-provider: interface điều khiển loại OpenFlow switch cụ thể

- netdev: library định nghĩa (abstracts) network devices

- netdev-provider:  OS- and hardware-specific interface to network devices

1.2.  Key Data Structures
--------------------

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-44-55.png

- OVS bridges quản lý hai loại tài nguyên:

- - forwarding plane mà nó điều khiển (datapath)

- - (physical and virtual) network devices được gắn vào nó (netdev)

- Các cấu trúc dữ liệu chính:

- - Triển khai OVS bridge     :ofproto, ofproto-provider

- - để quản lý đường datapath : dpif, dpif-provider

- - để quản lý network device : netdev, netdev-provider

1.2.1. ofproto
------------

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-45-14.png

- struct ofproto abstracts OpenFlow switches. Một ofproto instance là một OpenFlow switch (bridge).

- Data Structures (ofproto/ofproto-provider.h):

- - struct ofproto: thể hiện một OpenFlow switch (ovs bridge), tất cả flow/port được thực hiện trên ofproto

- - struct ofport: thể hiện một port trong một ofproto

- - struct rule: thể hiện một OpenFlow flow trong một ofproto

- - struct ofgroup: thể hiện OpenFlow 1.1+ group trong một ofproto

1.2.2. ofproto-provider
----------------


ofproto class structure, được xác định bởi mỗi một triển khai của ofproto (ovs bridge).

Một ofproto provider là những gì ofproto sử dụng để giám sát và điều khiển trực tiếp một OpenFlow-capable switch. struct ofproto_class, trong ofproto / ofproto-provider.h, xác định các giao diện để triển khai một ofproto provider cho phần cứng hoặc phần mềm mới.

Open vSwitch có built-in ofproto provider tên là ofproto-dpif, được xây dựng ở phía trên để thao tác các datapath, được gọi là dpif. “datapath” là một flow table đơn giản.Khi một gói đến. datapath tìm kiếm nó trong flow table. Nếu có một kết quả phù hợp, thì nó sẽ thực hiện các hành động liên quan. Nếu không có kết quả phù hợp, datapath sẽ chuyển gói đến ofproto-dpif, nơi mà duy trì toàn bộ bảng luồng OpenFlow. Nếu gói phù hợp trong bảng luồng này, thì ofproto-dpif thực thi các hành động của nó và chèn một mục mới vào flow table của dpif. (Nếu không, ofproto-dpif chuyển gói đến ofproto để gửi gói OpenFlow controller, nếu như nó được cấu hình.)

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-45-25.png

1.2.3. netdev
-----------

- Open vSwitch library, định nghĩa trong lib/netdev-provider.h, triển khai trong lib/netdev.c, xác định cách tương tác với các network devices, nghĩa là Ethernet interfaces.

- Mỗi porttrên switch phải có một netdev tương ứng

1.2.4. netdev-provider
-------------

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-45-35.png

netdev provider triển khai OS- and hardware-specific interface to “network devices”, và ethernet device. Open vSwitch cần phải có khả năng mở mỗi port trên switch dưới dạng netdev, vì thế cần triển khai “netdev provider” hoạt động với hardware và software của switch.

**Tất cả các loại class của netdev:**

- linux netdev (lib/netdev-linux.c, for linux platform)

- - system - netdev_linux_class

- - tap - netdev_tap_class

- - internal - netdev_internal_class

- bsd netdev (lib/netdev-bsd.c, for bsd platform)

- - system - netdev_bsd_class

- - tap - netdev_tap_class

- windows netdev (for windows platform)

- - system - netdev_windows_class

- - internal - netdev_internal_class

- dummy netdev (lib/netdev-dummy.c)

- - dummy - dummy_class

- - dummy-internal - dummy_internal_class

- - dummy-pmd - dummy_pmd_class

- vport netdev (lib/netdev-vport.c, a vport holds a reference to a port in datapath, the latter could be opened with netdev_open())

- - tunnel class:

- - - geneve

- - - gre

- - - vxlan

- - - lisp

- - - stt

- - patch - patch_class

- dpdk netdev

- - dpdk_class

- - dpdk_ring_class

- - dpdk_vhost_class

- - dpdk_vhost_client_class

1.3. Call Flows
------------

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-45-51.png

- Bắt đầu, khởi tạo bridge module, lấy một số tham số cấu hình từ ovsdb

- Sau đó, ovs-vswitchd đi vào main loop. vòng lặp đầu tiên khởi tạo một số library, bao gồm DPDK và quan trọng nhất là ofproto

- Tiếp theo, mỗi datapath sẽ thực hiện công việc của mình bằng cách chạy ofproto_type_run (), nó sẽ gọi vào việc triển khai type_run () cụ thể của kiểu datapath đó

- Mỗi bridge sẽ thực hiện công việc của mình bằng cách chạy ofproto_run (), nó sẽ gọi vào việc triển khai run () cụ thể của ofproto class

- ovs-vswitchd sẽ xử lý các thông báo IPC (JSON-RPC), đến từ dòng lệnh (ovs-appctl) và ovsdb-server

- netdev_run () được gọi để xử lý tất cả các loại netdev khác nhau

- Sau khi tất cả các công việc trên được thực hiện, bridge module,  unixctl server, và netdev modules sẽ chuyển sang trạng thái chặn cho đến khi các tín hiệu mới kích hoạt


2. OVSDB
------------

2.1. OVSDB Overview
------------

ovsdb-server cung cấp RPC interfaces cho một hoặc nhiều Open vSwitch databases (OVSDBs). Nó hỗ trợ JSON-RPC client connections qua TCP/IP hoặc Unix domain sockets (active hoặc passive). Mỗi file OVSDB có thể được chỉ định trên dòng lệnh làm cơ sở dữ liệu. Nếu không có gì được chỉ định, mặc định là /etc/openvswitch/conf.db.

OVSDB nắm giữ các cấu hình switch-level:

- thông tin các bridges, interfaces, tunnel

- địa chỉ của OVSDB và OpenFlow controller 

Cấu hình được lưu trữ trên đĩa và vẫn tồn tại khi khởi động lại.

Các thuộc tính Custome database:

- value constraints

- weak references

- garbage collection

CLI:

- ovs-vsctl: sửa đổi DB bằng cách định cấu hình ovs-vswitchd

- ovsdb-tool: Quản lý DB, ví dụ: tạo / nén / chuyển đổi DB, hiển thị nhật ký DB

2.2. Key Data Structures
------------

- ovsdb_schema

- ovsdb

- ovsdb_server

- ovsdb_table_schema

- ovsdb_table

2.2.1. OVSDB
---------

2.2.2. OVSDB Table
-------------

ovsdb core tables:

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-46-05.png

Open_vSwitch là root table và luôn luôn chỉ có một dòng duy nhất

2.2.3. Flow Diagram
--------------

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-46-15.png

3. kernel module (datapath)
-------------

3.1. Overview
------------------

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-46-32.png

Datapath là forwarding plane của OVS. Ban đầu nó được triển khai như kernel module. Ngoài datapath được triển khai ở kernel space thì các thành phần khác được triển khai ở user space và có ít phụ thuộc vào nền tảng hệ thống. Điều đó có nghĩa là việc chuyển OVS sang các OS hay platform khác là rất đơn giản (về mặt lý thuyết): chỉ cần triển khai lại phần kernel trên OS hay platform mới

Thực tế các phiên bản gần đây OVS đã có 2 loại datapath để có thể chọn: kernel datapath và userspace datapath.

Open vSwitch hỗ trợ các datapath khác nhau trên các platform khác nhau:

- Linux upstream:
datapath được triển khai bởi kernel module được vận chuyển với Linux upstream. Các tính năng dần được đưa vào kernel

- Linux OVS tree:
datapath được triển khai bởi kernel module được phân phối với OVS source tree 

- Userspace:
Còn được gọi là DPDK, dpif-netdev hoặc dummy datapath. Đây là đường dẫn dữ liệu duy nhất hoạt động trên NetBSD và FreeBSD.

- Hyper-V:
Còn được gọi là Windows datapath    

3.1.1. Kernel datapath
------------

Trên linux, kernel datapath là loại datapath mặc định

Ví dụ lệnh tạo OVS bridge:

::

  $ ovs-vsctl add-br br0

  $ ovs-vsctl show
  05daf6f1-da58-4e01-8530-f6ec0d51b4e1
      Bridge br0
          Port br0
              Interface br0
                  type: internal
                  
3.1.2. Userspace Datapath
-------------------

Userspace datapath khác với datapath truyền thống ở chỗ việc chuyển tiếp và xử lý gói tin của nó được thực hiện trong userspace. Trong số đó, netdev-dpdk là một trong những cách triển khai, được hỗ trợ kể từ OVS 2.4.

Lệnh để tạo OVS bridge sử dụng userspace datapath:

::

  $ ovs-vsctl add-br br0 -- set Bridge br0 datapath_type=netdev
  
Lưu ý chỉ định rõ datapath_type là netdev khi tạo bridge, nếu không sẽ gặp lỗi ovs-vsctl: Error detected while setting up ‘br0’.    

**Official Doc**

Open vSwitch kernel module cho phép kiểm soát userspace linh hoạt đối với flow-level packet processing trên các thiết bị mạng được chọn. Nó có thể được sử dụng để triển khai Ethernet switch, network device bonding, VLAN processing, network access control, flow-based network control, v.v.

Kernel module triển khai nhiều datapath (tương tự như bridge), mỗi chúng có thể có nhiều vport (tương tự với các port trong bridge).

Khi một gói tin đến vport, kernel module sẽ xử lý nó bằng cách trích xuất flow key của nó và tra cứu nó trong flow table. Nếu có một luồng phù hợp, nó sẽ thực hiện các hành động liên quan. Nếu không trùng khớp, nó sẽ xếp hàng đợi gói đến userspace để xử lý (như một phần của quá trình xử lý, userspace có thể sẽ thiết lập một luồng để xử lý thêm các gói cùng loại hoàn toàn trong kernel).

.. figure:: https://github.com/thang140398/Lab/blob/master/protocol/picture/Screenshot%20from%202020-10-21%2018-46-45.png

3.2. Key Data Structures
--------------

- datapath - flow-based packet forwarding/swithcing module

- flow

- flow_table

- sw_flow_key

- vport

3.3. vport
------

Các kiểu:

- netdev

.send = dev_queue_xmit

dev_queue_xmit(skb) cuối cùng sẽ truyền gói tin trên thiết bị mạng vật lý

- internal

.send = internal_dev_recv

send method sẽ gọi netif_rx(skb) chèn skb vào TCP/IP stack, và gói cuối cùng sẽ được truyền theo ngăn xếp

- patch

.send = patch_send()

ssend method sẽ chỉ chuyển skb pointer đến vport ngang hàng

- tunnel vports: vxlan, gre, ...

tunnel xmit method in kernel, e.g. .send = vxlan_xmit for vxlan



*Tham khảo:*
http://arthurchiao.art/blog/ovs-deep-dive-0-overview/








