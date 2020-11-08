Cài đặt FRRouting từ source
================================

Chuẩn bị
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-   Máy ảo cấu hình: 2 CPU, 2GB RAM, 2 card mạng (1 NAT và 1 private)
-   Hệ điều hành: Ubuntu Server 18.04
-   Máy ảo có kết nối internet

Cài đặt trên Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Chạy các lệnh sau

::

    #Install dependencies
    sudo apt update -y
    sudo apt-get install -y \
    git autoconf automake libtool make libreadline-dev texinfo \
    pkg-config libpam0g-dev libjson-c-dev bison flex python3-pytest \
    libc-ares-dev python3-dev libsystemd-dev python-ipaddress python3-sphinx \
    install-info build-essential libsystemd-dev libsnmp-dev perl libcap-dev

    #install git
    sudo apt install -y git

    #Source install
    sudo apt install -y cmake
    sudo apt-get install -y libpcre3-dev libpcre3

    git clone https://github.com/CESNET/libyang.git
    cd libyang
    mkdir build; cd build
    cmake -DENABLE_LYD_PRIV=ON -DCMAKE_INSTALL_PREFIX:PATH=/usr \
        -D CMAKE_BUILD_TYPE:String="Release" ..
    make
    sudo make install

    #Protobuf
    sudo apt-get install -y protobuf-c-compiler libprotobuf-c-dev

    #ZeroMQ
    sudo apt-get install -y libzmq5 libzmq3-dev

    #Building & Installing FRR
    #Add FRR user and groups

    sudo groupadd -r -g 92 frr
    sudo groupadd -r -g 85 frrvty
    sudo adduser --system --ingroup frr --home /var/run/frr/ \
    --gecos "FRR suite" --shell /sbin/nologin frr
    sudo usermod -a -G frrvty frr

    #compile
    git clone https://github.com/frrouting/frr.git frr
    cd frr
    ./bootstrap.sh
    ./configure \
        --prefix=/usr \
        --includedir=\${prefix}/include \
        --enable-exampledir=\${prefix}/share/doc/frr/examples \
        --bindir=\${prefix}/bin \
        --sbindir=\${prefix}/lib/frr \
        --libdir=\${prefix}/lib/frr \
        --libexecdir=\${prefix}/lib/frr \
        --localstatedir=/var/run/frr \
        --sysconfdir=/etc/frr \
        --with-moduledir=\${prefix}/lib/frr/modules \
        --with-libyang-pluginsdir=\${prefix}/lib/frr/libyang_plugins \
        --enable-configfile-mask=0640 \
        --enable-logfile-mask=0640 \
        --enable-snmp=agentx \
        --enable-multipath=64 \
        --enable-user=frr \
        --enable-group=frr \
        --enable-vty-group=frrvty \
        --with-pkg-git-version \
        --with-pkg-extra-version=-MyOwnFRRVersion
    make
    sudo make install

    #Install FRR configuration files
    sudo install -m 775 -o frr -g frr -d /var/log/frr
    sudo install -m 775 -o frr -g frrvty -d /etc/frr
    sudo install -m 640 -o frr -g frrvty tools/etc/frr/vtysh.conf /etc/frr/vtysh.conf
    sudo install -m 640 -o frr -g frr tools/etc/frr/frr.conf /etc/frr/frr.conf
    sudo install -m 640 -o frr -g frr tools/etc/frr/daemons.conf /etc/frr/daemons.conf
    sudo install -m 640 -o frr -g frr tools/etc/frr/daemons /etc/frr/daemons


Sửa file /etc/sysctl.conf

.. figure:: https://user-images.githubusercontent.com/41882267/98118250-8a30ed00-1edd-11eb-8faa-62c34840b52a.png
   :alt: FRRouting



Sử dụng lệnh sau để áp dụng thay đổi trong file sysctl.conf
::

   sudo sysctl -p


Thêm MPLS kernel modules bằng cách sửa file /etc/modules-load.d/modules.conf

.. figure:: https://user-images.githubusercontent.com/41882267/98448577-8c09e300-215f-11eb-984c-595b5a8f1209.png
   :alt: FRRouting


Load lại kernel modules bằng lệnh
::

    sudo modprobe mpls-router mpls-iptunnel

Bật MPLS Forwarding bằng cách thêm các dòng sau vào file /etc/sysctl.conf

.. figure:: https://user-images.githubusercontent.com/41882267/98448639-0aff1b80-2160-11eb-8783-2baef09d5071.png
   :alt: FRRouting

Lưu ý: ens33 là tên 1 card mạng trên máy được cài đặt, có thể thay đổi tuỳ vào máy đó. Nếu máy có nhiều hơn 1 card mạng, muốn enable thêm card nào thì thêm dòng với cú pháp tương tự.

Sử dụng lệnh sau để áp dụng thay đổi trong file sysctl.conf
::

   sudo sysctl -p

Install service files bằng lệnh

::

    sudo install -m 644 tools/frr.service /etc/systemd/system/frr.service

.. figure:: https://user-images.githubusercontent.com/41882267/98448729-dc357500-2160-11eb-9e4d-12c6e81b3a67.png
   :alt: FRRouting

Lưu ý: Phải chạy lệnh trên trong folder của frr nếu không sẽ bị báo lỗi

Enable và start frr bằng lệnh
::
    sudo systemctl enable frr
    sudo systemctl start frr

Kiểm tra frr bằng lệnh
::

    sudo systemctl status frr

.. figure:: https://user-images.githubusercontent.com/41882267/98448777-45b58380-2161-11eb-836d-32f4cb2ba943.png
   :alt: FRRouting

Nếu status của frr như trên tức là đã cài đặt thành công frr.