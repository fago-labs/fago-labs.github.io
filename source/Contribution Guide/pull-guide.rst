Hướng dẫn pull request cho github của blog page FAGO.LABS
============================================================

*Địa chỉ github: https://github.com/fago-labs/fago-labs.github.io*

Fork repository trên Github
---------------------------------

Tạo một bản copy của repository như sau. Truy cập vào repository theo link trên. Click vào button **Fork** ở góc phải màn hình

.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/Screenshot%20from%202020-11-15%2017-12-03.png

Quá trình fork sẽ được thực hiện và Kết quả là đã fork thành công repository về tài khoản của mình

.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/Screenshot%20from%202020-11-15%2017-12-29.png

Clone về máy
-----------------------------

Ví dụ
::

  git clone https://github.com/thang140398/fago-labs.github.io



Tạo file HTML bằng Sphinx
-------------------------------

*Cần cài đặt Sphinx trước*

cd và folder fago-labs.github.io

Các file .rst, .md sẽ để trong folder **source**

Nếu cần thêm các mục thì thêm vào file **index.rst**

.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/Screenshot%20from%202020-11-15%2017-32-14.png

.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/Screenshot%20from%202020-11-15%2017-27-28.png

Dùng lệnh sau để tạo file HTML

::

  sphinx-autobuild source/ docs/
  
.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/125041655_417251322606927_2753747689888614615_n.png

Xem trang HTML vừa tạo qua địa chỉ local : 127.0.0.1

.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/125431154_784353335455051_3716113982632786603_n.png

Tạo push lên github
------------------------------

Ví dụ
::

  git remote set-url https://github.com/thang140398/fago-labs.github.io.git

  git status

  git add .

  git commit -m "Thang update ovs"

  git push origin master
  
Tạo push request như sau

- click vào pull request

.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/Screenshot%20from%202020-11-15%2017-38-15.png

- click vào create

.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/Screenshot%20from%202020-11-15%2017-39-31.png

- Đặt tên và click vào create

.. figure:: https://raw.githubusercontent.com/thang140398/Lab/master/IMG_for_GuildPull/Screenshot%20from%202020-11-15%2017-40-10.png














