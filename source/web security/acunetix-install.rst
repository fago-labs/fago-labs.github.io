Cách cài đặt Acunetix
==========================

1. Yêu cầu phần cứng
-----------------------
-   Cấu hình: 2 CPU 4GB RAM
-   Hệ điều hành: Ubuntu, Kali Linux...
-   VMware Worstation 15.5
-   Kết nối mạng

2. Cài đặt Acunetix
-------------------

Cài đặt trên Ubuntu và Kali Linux
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Chạy lệnh sau để cài đặt Acunetix

::

    apt install unzip curl ; rm -rf /tmp/acun*
    apt install libxdamage1 libgtk-3-0 libasound2 libnss3 libxss1 libx11-xcb1 -y
    cd /tmp; rm master.zip -f
    curl -L -o master.zip http://github.com/neolead/acunetix-linux/zipball/master/
    unzip master.zip
    cd `ls|grep neolead` && cat acupatch* > acupatch.tgz
    tar -zxvf acupatch.tgz
    chmod +x ./acunetix_trial.sh
    ./acunetix_trial.sh
    service acunetix_trial stop
    chmod 0755 patch_awvs
    cp patch_awvs /home/acunetix/.acunetix_trial/v_190515149/scanner/
    su - acunetix -c "/home/acunetix/.acunetix_trial/v_190515149/scanner/patch_awvs"
    cd /tmp;rm -rf /tmp/acu*
    service acunetix_trial stop
    chattr +i /home/acunetix/.acunetix_trial/data/license/license_info.json
    service acunetix_trial start
    curl -k https://127.0.0.1:13443
    su - acunetix -c "/home/acunetix/.acunetix_trial/ && bash change_credentials.sh"

Sau khi quá trình cài đặt xong, truy cập vào Acunetix qua địa chỉ http://127.0.0.1:13443

Cài đặt trên Window 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download Acunextic tại link  https://a236.x8top.net/v2106xm/3/4/acunetix-web-vulnerability-scanner.exe

Sau khi download, click vào file setup để cài đặt

.. figure:: https://user-images.githubusercontent.com/32956424/98446115-02054e80-214e-11eb-8474-efc4cee279c1.png
   :alt: Acunetix
   

Chọn **I accepted with agreement**

.. figure:: https://user-images.githubusercontent.com/32956424/98446143-2e20cf80-214e-11eb-8213-3e1f6b52754e.png
   :alt: Acunetix

Nhập thông tin email và password để đăng ký tài khoản

.. figure:: https://user-images.githubusercontent.com/32956424/98446180-57d9f680-214e-11eb-951a-be472173ff7c.png
   :alt: Acunetix
   
Điền số port và chọn **Next**
   
.. figure:: https://user-images.githubusercontent.com/32956424/98446193-69230300-214e-11eb-8e19-57bafe31997b.png
   :alt: Acunetix
   
Cài đặt hoàn tất
   
.. figure:: https://user-images.githubusercontent.com/32956424/98446204-78a24c00-214e-11eb-9a49-fff50346e1a4.png
   :alt: Acunetix
   
Sau khi quá trình cài đặt xong, truy cập vào Acunetix qua địa chỉ https://localhost:13443/
 
Giao diện của Acunetix
 
.. figure:: https://user-images.githubusercontent.com/32956424/98449148-bc538080-2163-11eb-849d-5b99ab439aa7.png
   :alt: Acunetix
