Monitoring - Alerting Kubernetes với Prometheus và Grafana
==============================================================

.. contents:: Nội dung

1. Prometheus
-----------------------

Prometheus là một bộ công cụ giám sát và cảnh báo hệ thống mã nguồn mở ban đầu được xây dựng bởi công ty SoundCloud. Được thành lập vào năm 2012, nhiều công ty và tổ chức đã áp dụng Prometheus vào hệ thống và dự án này có một cộng đồng người dùng và nhà phát triển rất tích cực.

Prometheus bây giờ đã trở thành một dự án mã nguồn mở độc lập và được duy trì độc lập với bất kỳ công ty nào. Prometheus đã tham gia vào tổ chức Cloud Native Computing Foundation vào năm 2016 với tư cách là dự án được ưu tiên phát triển lớn thứ hai, sau Kubernetes (K8s).

Prometheus có khả năng thu thập thông số/số liệu (metric) từ các mục tiêu được cấu hình theo các khoảng thời gian nhất định, đánh giá các biểu thức quy tắc, hiển thị kết quả và có thể kích hoạt cảnh báo nếu một số điều kiện được thỏa mãn yêu cầu.



2. Grafana
-------------------------------

Grafana là một giao diện/dashboard theo dõi hệ thống (open source), hỗ trợ rất nhiều loại dashboard và các loại graph khác nhau để người quản trị dễ dàng theo dõi.

Grafana có thể truy xuất dữ liệu từ Graphite, Elasticsearch, OpenTSDB, Prometheus và InfluxDB. Grafana là một công cụ mạnh mẽ để truy xuất và biểu diễn dữ liệu dưới dạng các đồ thị và biểu đồ.


3. Cài đặt Prometheus và Grafana
----------------------------------

Helm
~~~~~~~~~~~~~~~~~~~~~~~~~

Helm là package manager của Kubernetes, dùng để quản lý các Kubernetes application được dựng lên từ những Kubernetes resource. Cài đặt với Helm sẽ đơn giản hơn thay vì viết từng file yaml

Trên Master node, chạy lệnh sau để cài đặt Helm
::

    curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
    sudo apt-get install apt-transport-https --yes
    echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
    sudo apt-get update
    sudo apt-get install helm

Sau khi cài xong, chạy lệnh **helm version** để kiểm tra version của Helm


Cài đặt Prometheus và Grafana bằng Helm
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Thêm Repo cho Helm
::

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo add stable https://kubernetes-charts.storage.googleapis.com/
    helm repo update


Tạo namespace cho monitoring
::

    kubectl create namespace monitoring

Cài đặt chart
::

    helm install --namespace monitoring prome-grafana prometheus-community/kube-prometheus-stack

Expose Service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Để sử dụng được dịch vụ của Prometheus và Grafana, cần expose dịch vụ ra ngoài internet. Sử dụng Node Port. Dùng lệnh get pod để xem các pod của Prometheus chạy trên node nào
::

    root@master:~# kubectl get pods --namespace monitoring -o wide

.. figure:: https://lh5.googleusercontent.com/adGP0G4NOJXjG1eXPF91DmwEwY2tiDd_2Ng5AiI2r2ei4IfFVa4Deq7QrK1ea4ECux_qEUjyaaX4YQ7vGn_nBDMnhUd6hddqSqYX_15LVIovPtFQ_ZR-pWlxbtH8SXwagLYboPQ

Dùng lệnh get service để xem các dịch vụ
::

    root@master:~# kubectl get svc --namespace monitoring -o wide

.. figure:: https://lh4.googleusercontent.com/1qC6feNUOQT8W75OAkog7bTHt9apWI5BUdEZ5RLEuymXxdLxNsZ0WhPBKBBgK5ZAZCfEWhp3fHfDLEo9F49oi1wa6_-KAA77f3g_cSrdWL0zjYmPeT2tVbkasRZ6J_xjEXAA7Fk

Có 1 service cần lưu ý là: prome-grafana. Đây là service của Grafana. Tiến hành chỉnh sửa service bằng lệnh edit svc
::

    kubectl edit svc prome-grafana --namespace monitoring

.. figure:: https://lh3.googleusercontent.com/igu-eQgmgO2NbRG3C5h2sGZcw-eS2F4aQwLajE-avyEe3eJQCWtvIVDaP6crtjd0v1IU5hd6N5-L7KXVLXwDrFkdlX8NLND9mN40w1tJ8G1R1FUW5uTU6NgAInkLSWuQYhxswmI

Sửa mục type của service từ ClusterIP thành NodePort và lưu lại. Dùng lệnh describe svc để xem NodePort của service đó
::

    kubectl describe svc  prome-grafana --namespace monitoring

.. figure:: https://lh6.googleusercontent.com/98CIQ0Hp2W3m0Vw5LmJePh-WhFbu6oBhfPgWfNaIeQngf3_I5nN_ISwn-WnkZWBvA2ZvD4rY5i1EgvYk8K_tHSA_-Emanvv7Vz_FFPoq34LRCvREW4vKL2nMSjhwa_omUVsEYVQ

NodePort của service prome-grafana là **31931**

Truy cập vào service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Chạy lệnh get pod để xem các service chạy trên Node nào
::

    kubectl get pods --namespace monitoring -o wide

.. figure:: https://lh4.googleusercontent.com/8IZO6Xmz2JvDq71bcjizJSbSUxn2ih6b5P_TjxW8UrHRBc6ZGYOeDa4QFu5LuheBH5nBP5FtOKas8oLYCRPUdkrZNLa6XxvsbguPwaD-8iM7zEGlC8A1upTY-9xyLrQojaZdtNk

Service prome-grafana có Endpoint là 192.168.189.72:3000 -> node Worker2

Truy cập vào Grafana qua địa chỉ http://35.247.146.200:31931 

.. figure:: https://lh3.googleusercontent.com/B8g8MevVCSjA2Z6ylr3ywzqr46YXC43BkhBKMaZSB4abM4Qwh0VLLwYKzMyo4CsK-YAaAMktWVynfkMi1tWucQ4e_vTIr-R7zPzJUzNiSl6S2nT0mJrENSvjG7J1BDW7cPUpsws


4. Monitoring và Alerting
----------------------------------

Prometheus dùng các trình daemon cài sẵn trên các node để thu thập các thông tin cần thiết, giao tiếp với máy chủ quản lý monitor qua giao thức HTTP/HTTPs và lưu trữ data theo dạng time-series database (TSDB).

Tuy nhiên, sử dụng Grafana sẽ có giao diện đẹp hơn, chia thành nhiều mục để người dùng dễ dàng hơn trong việc đánh giá hiệu suất của cluster. Grafana sẽ lấy dữ liệu metric thu được từ Prometheus, phân tích và tạo ra dashboard mô tả trực quan các thông số như CPU, RAM, Disk, Network…

.. figure:: https://lh6.googleusercontent.com/n79bCXboJKBtZhEqK2La6qtw8Owc3Z_Hc0DjxsKD_zZa13x6AuZQjX5npqd2UhH6B9W9JE_U66mnmwEteKN3BMIh_SHQQOsrLfgNy2ZZ5CdVyIGogJB4m6-Vrh6XBhhoyWLITlU

Thống kê chi tiết các dashboard của Grafana Người dùng có thể chọn các dashboard của Grafana

.. figure:: https://lh4.googleusercontent.com/ahMakVpEhxX5wRG4pjhOcT7PRYSCiDF8gmB7o-D6KDqQx-7_qzuDUmvJ7vtHyuhRSUIIMX11KCISLuk7b-GcRXCBv1Y2Iin3Zu8j4OQWmypvcJiGAQ08gwMIDXvABfuYKXSmTiE

Alert giúp người dùng xác định những vấn đề bất thường xảy ra trong hệ thống. Bằng cách tùy chỉnh những thay đổi, người dùng có thể giảm thiểu sự gián đoạn trong hệ thống. Alert gồm 2 phần:

- Alert Rule: Là điều kiện để Alert được kích hoạt, Alert Rule được người dùng định nghĩa bằng 1 hoặc nhiều điều kiện
- Notification channel: Cách mà Alert được gửi đi, khi điều kiện của Alert được kích hoạt. Grafana sẽ gửi sẽ gửi thông báo cho channel. Notification channel có thể là Email, Telegram, Discord...

Grafana 4.0 trở lên hỗ trợ cấu hình Alert

Notification Channel Discord
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Discord là hệ thống trò chuyện bằng giọng nói và văn bản, cho phép giao tiếp với những người khác. Bất cứ ai cũng có thể tạo ra một máy chủ lưu trữ thảo luận với mọi người. Discord có mặt trên các hệ điều hành như Window, Ubuntu, Mac OS, Android…

Sau khi đăng ký tài khoản Discord, tiến hành tạo 1 server Discord - đây là nơi mà Alert từ Grafana sẽ gửi về. Đặt tên cho server

.. figure:: https://lh6.googleusercontent.com/Ezi2LInMm97R6_F31TFdO8oes5W0SI50wxxmeYaToI2X0LFPw8xvgN1SSHuselihW2tb4SCQMSht2-DSG0CM3WhBxDPhfSn8r_Feuk5d0xZ7f1jJ7lB8UkL_I8vflmvQ8mQD1WM

Vào Server Setting -> Integration. Tạo 1 Webhook cho Server

.. figure:: https://lh6.googleusercontent.com/aHDvg2urecOLjhrhWJJFyLQnxTNvdiC9f_0_Wv99oR5-bVbtLD17E2rrTtqPO62rhtgf9D01Y6XsVVJoP8kZzdbyzi4eyKumWC2LK-76wVXaysBiVPCMxG2kY6lDugx_3TFcv74

Đặt tên cho Webhook và copy Webhook URL

.. figure:: https://lh3.googleusercontent.com/HNMauFllnDwfckORYc5052R174hpwIM2i8LPeVjvXoQhT2pHe6Tnb7FlvgkqK8zg1S00rsvlI9SkaywfDDCzeozil2OFMCyKUXHj1EDZ8ZkPIua8NxmpVe5CDjDLCPWwreFF8eg


Truy cập vào phần Alert trên Grafana, chọn New Channel để tạo Notification Channel. Đặt tên cho Channel, chọn type Discord, paste Webhook URL của Discord đã copy trước đó vào phần Webhook URL

.. figure:: https://lh6.googleusercontent.com/p6K8cb2nRoNJcMYFX0TdKmo3YJh48NRY5NpEflXROvKIXJjrD4YYB2NqlTHKRLsL_VCys68a3UWLM6mWynkxATASPsZRl_psCqXQ1M0wNpPYYcEAzwqyastPOpsL1GdtS3-N8TM

Để kiểm tra Notification có hoạt động hay không, click vào Test. Grafana sẽ gửi 1 Alert Test về Discord server

.. figure:: https://lh6.googleusercontent.com/nORGdWG0gR3bkqwVj37REWRoxTjrybp6s9xqB8CMRfrHACmpwoEdpYoWFDKWKdacclWcxNyC6JMJl9xFaVM8pYGMuw2PIIkLvvFguzkc8W9J0GOzKbrPJwEXIo2n4fxoxluVUdA









































































