Cách cài đặt FRRouting
======================

Yêu cầu phần cứng
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-   Máy ảo cấu hình: 2 CPU 2GB RAM 2 card mạng (1 NAT và 1 private)
-   Hệ điều hành: Ubuntu Server 18.04
-   Có kết nối internet

Cài đặt trên Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Chạy lệnh sau để cài đặt FRRouting

::

   sudo apt install -y curl
   sudo apt update -y
   sudo apt install -y gnupg2
   # add GPG key
   curl -s https://deb.frrouting.org/frr/keys.asc | sudo apt-key add -
   # possible values for FRRVER: frr-6 frr-7 frr-stable
   # frr-stable will be the latest official stable release
   FRRVER="frr-stable"
   echo deb https://deb.frrouting.org/frr $(lsb_release -s -c) $FRRVER | sudo tee -a /etc/apt/sources.list.d/frr.list
   # update and install FRR
   sudo apt update -y && sudo apt install -y frr frr-pythontools



Sửa file /etc/sysctl.conf

.. figure:: https://user-images.githubusercontent.com/41882267/98118250-8a30ed00-1edd-11eb-8faa-62c34840b52a.png
   :alt: FRRouting



Sử dụng lệnh sau để đọc lại file sysctl.conf
::

   sysctl -p


.. figure:: https://user-images.githubusercontent.com/41882267/98118342-ac2a6f80-1edd-11eb-9148-5e0ace19a8fe.png
   :alt: FRRouting



Kiểm tra trạng thái của FRRouting bằng lệnh
::

   systemctl status frr

Nếu kết quả trả về active (running) tức là thành công

.. figure:: https://user-images.githubusercontent.com/41882267/98118535-f0b60b00-1edd-11eb-9110-ea54da7f0f47.png
   :alt: FRRouting
