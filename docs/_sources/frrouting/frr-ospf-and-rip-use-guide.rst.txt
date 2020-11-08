Cách sử dụng FRRouting
======================

Chuẩn bị
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-   2 Máy ảo, cấu hình mỗi máy: 2 CPU, 2GB RAM, 2 card mạng (1 NAT và 1 private)
-   Hệ điều hành: Ubuntu Server 18.04
-   2 máy ảo đều đã được cài đặt FRRouting theo cách trong file frr-install-guide

OSPF
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
2 máy ảo chuẩn bị tương ứng với 2 router trong mô hình sau

.. figure:: https://user-images.githubusercontent.com/41882267/98120853-14c71b80-1ee1-11eb-8663-476ecfd7095c.png
   :alt: FRRouting

Để bật chế độ ospf, trên mỗi router, sửa "ofpfd=yes" trong file /etc/frr/daemons

.. figure:: https://user-images.githubusercontent.com/41882267/98121727-3ecd0d80-1ee2-11eb-9105-ffb94e913ada.png
   :alt: FRRouting

Restart frr để nhận thay đổi
::

   systemctl restart frr

Kiểm tra trạng thái của FRRouting bằng lệnh
::

   systemctl status frr

Nếu kết quả trả về active (running) và có ofpfd tức là thành công

.. figure:: https://user-images.githubusercontent.com/41882267/98121440-df6efd80-1ee1-11eb-8493-f4e73f705e01.png
   :alt: FRRouting

Kiểm tra 2 router trước khi sử dụng ospf bằng lệnh
::

   netstat -nr

.. figure:: https://user-images.githubusercontent.com/41882267/98122090-b3a04780-1ee2-11eb-924e-dede1fdc2879.png
   :alt: FRRouting

.. figure:: https://user-images.githubusercontent.com/41882267/98122141-c3b82700-1ee2-11eb-9d07-de85756a6f14.png
   :alt: FRRouting


Để vào frr, sử dụng lệnh
::

   vtysh

Tiến hành cấu hình router 1 như sau (router 2 tương tự)

Vào config terminal của router bằng lệnh
::

    conf t

Vào chế độ ospf bằng lệnh
::

    router ospf

Thêm 2 network cùng area 0 tương ứng với 2 card mạng như trong mô hình
::

    network 172.16.69.0/24 area 0
    network 192.168.182.0/24 area 0

Thoát config terminal bằng lệnh
::

    end

Tiến hành ospf route bằng lệnh
::

    sh ip ospf route

Router 1

.. figure:: https://user-images.githubusercontent.com/41882267/98196879-e20c3a00-1f57-11eb-8121-5e0a993e377c.png
   :alt: FRRouting

Router 2

.. figure:: https://user-images.githubusercontent.com/41882267/98196916-fe0fdb80-1f57-11eb-8046-67a526a5c56b.png
   :alt: FRRouting




Kết quả cho thấy 2 router đã trao đổi bảng định tuyến với nhau.

Sau khi thực hiện trao đổi bảng định tuyến, kiểm tra lại 2 router bằng lệnh
::

   netstat -nr

.. figure:: https://user-images.githubusercontent.com/41882267/98124054-4215c880-1ee5-11eb-9bc5-c1ea9f516bb1.png
   :alt: FRRouting

.. figure:: https://user-images.githubusercontent.com/41882267/98124160-62458780-1ee5-11eb-9434-9b26d4a1f0d6.png
   :alt: FRRouting

Để loại bỏ đi các network vừa thêm, sử dụng cú pháp
::

   no network <network> area 0

Tiến hành ospf route lại như sau

Router 1

.. figure:: https://user-images.githubusercontent.com/41882267/98196989-2dbee380-1f58-11eb-9941-b47dddf64002.png
   :alt: FRRouting

Router 2

.. figure:: https://user-images.githubusercontent.com/41882267/98197025-43340d80-1f58-11eb-8fe0-5a82c8e6ee63.png
   :alt: FRRouting


Kiểm tra lại 2 router bằng lệnh
::

   netstat -nr

.. figure:: https://user-images.githubusercontent.com/41882267/98124965-6e7e1480-1ee6-11eb-9044-3beaa4361bac.png
   :alt: FRRouting

.. figure:: https://user-images.githubusercontent.com/41882267/98125048-86559880-1ee6-11eb-86f1-226e50702bf2.png
   :alt: FRRouting


RIP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
2 máy ảo chuẩn bị tương ứng với 2 router trong mô hình sau

.. figure:: https://user-images.githubusercontent.com/41882267/98120853-14c71b80-1ee1-11eb-8663-476ecfd7095c.png
   :alt: FRRouting

Tương tự với ospf, để bật rip, trên 2 router sửa "ripd=yes" trong file /etc/frr/daemons, sau đó restart frr và kiểm tra trạng thái của frr bằng lệnh

::

  systemctl status frr

.. figure:: https://user-images.githubusercontent.com/41882267/98125414-08de5800-1ee7-11eb-97f9-233d5fd635cd.png
   :alt: FRRouting

Vào frr bằng lệnh
::

   vtysh

Cấu hình rip router 1 (router 2 tương tự). Để vào chế độ config, sử dụng lệnh
::

   conf t

Để cấu hình router rip, sử dụng lệnh
::

   router rip

Router 1 có 2 card mạng như trong mô hình, vì vậy thêm 2 network tương ứng trên router 1 như sau:

::

   network 172.16.69.0/24
   network 192.168.182.0/24

Thoát ra bằng lệnh
::

   end

Ghi lại cấu hình vào file /etc/frr/frr.conf bằng lệnh
::

   write

Làm tương tự trên router 2, ta được như sau

.. figure:: https://user-images.githubusercontent.com/41882267/98127289-198fcd80-1ee9-11eb-9119-29a7d47a9e62.png
   :alt: FRRouting

.. figure:: https://user-images.githubusercontent.com/41882267/98127411-4348f480-1ee9-11eb-9a4c-880b6a68a315.png
   :alt: FRRouting


2 file /etc/frr/frr.conf lần lượt trên router 1 và router 2 có dạng như sau

Router 1

.. figure:: https://user-images.githubusercontent.com/41882267/98127620-7c816480-1ee9-11eb-9298-5b2b4b4dacc4.png
   :alt: FRRouting

Router 2

.. figure:: https://user-images.githubusercontent.com/41882267/98127793-acc90300-1ee9-11eb-9af4-da2a4dfdbc17.png
   :alt: FRRouting

Kiểm tra lại 2 router bằng lệnh
::

   netstat -nr

.. figure:: https://user-images.githubusercontent.com/41882267/98192816-580ba380-1f4e-11eb-9f11-8f73418f9ddf.png
   :alt: FRRouting

.. figure:: https://user-images.githubusercontent.com/41882267/98192898-8a1d0580-1f4e-11eb-8a30-d6eecb5d6e9c.png
   :alt: FRRouting

Để loại bỏ cấu hình rip, ta xoá các network vừa thêm trên các router, ta sử dụng cú pháp
::

   no network <network>

Router 1

.. figure:: https://user-images.githubusercontent.com/41882267/98194103-37911880-1f51-11eb-9fc7-2d6e747d1ba4.png
   :alt: FRRouting

Router 2

.. figure:: https://user-images.githubusercontent.com/41882267/98194168-51326000-1f51-11eb-9d0c-15424e0b49c7.png
   :alt: FRRouting

Kiểm tra lại 2 router bằng lệnh

netstat -nr

.. figure:: https://user-images.githubusercontent.com/41882267/98194247-83dc5880-1f51-11eb-9d8e-379a7ace91e4.png
   :alt: FRRouting

.. figure:: https://user-images.githubusercontent.com/41882267/98194297-a0789080-1f51-11eb-91ec-04278e857f2e.png
   :alt: FRRouting
   
