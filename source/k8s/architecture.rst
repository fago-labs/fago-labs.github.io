Kiến trúc Kubernetes
==========================================

.. contents:: Nội dung

1. Kiến trúc cơ bản của Kubernetes
-------------------------------------

.. figure:: https://lh5.googleusercontent.com/gAl9IfHycRc19aM4Qky_VVopcpJrwVbcl93-C-WyWFMCx4KGe9J-BVHJjF43eRUrI5Jel-Fsm1QaeX0Z7B_kT_97UsmENQu5MXw33ZqTyODiyn4plmKCDMACOiQnXYMxkJ7ulRc

Ở góc độ đơn giản nhất, Kubernetes bao gồm 2 thành phần chính là: Master node và Worker node. Các node ở đây có thể là máy ảo, máy vật lý…

**Master** là thành phần điều khiển toàn bộ các hoạt động chung và kiểm soát các container các node Worker, triển khai các ứng dụng trên node Worker, phần lớn thao tác của người quản trị sẽ được thực hiện trên Master node.

**Worker** là nơi chạy các ứng dụng do Master chỉ định

Cả Master và Worker đều có thể chạy trên máy chủ thật hoặc máy chủ ảo. Một cụm Kubernetes (Kubernetes Cluster) có thể bao gồm từ vài chục máy cho đến hàng nghìn máy chủ.

Các lập trình viên chỉ cần viết ứng dụng, viết bản mô tả cho ứng dụng, rồi đưa bản mô tả cho Master. Master sẽ điều khiển các Worker chạy ứng dụng này. Về phía người dùng sẽ truy cập trực tiếp đến ứng dụng ở Worker, các Worker sẽ xử lý yêu cầu của người dùng

2. Một số khái niệm của Kubernetes
-------------------------------------

**Kubernetes Cluster**: là một cụm các máy chủ vật lý hoặc máy chủ ảo được sử dụng bởi Kubernetes để chạy ứng dụng, bao gồm Master Node và Worker Node

.. figure:: https://lh6.googleusercontent.com/BoyWAknaS0T_AScwBv4kQq3KqSxWhs_cD7eFTEoMrda5R35BV16bMSd424SC41cjKBCrpkerDMgOuTtuT7HNLBKcq0SBH_G24RyqYX6cvNE7VgGfl_pJVPx2E000ZHp0rO8MEmQ

**Node**: Một node là một máy ảo hoặc máy vật lý chạy Kubernetes, còn gọi là docker host

.. figure:: https://lh5.googleusercontent.com/dU4iS8SWkQLPyBSWGhF89FC1byf9OykPPVacYw2PnZsTelbRQqPVg2e7K4fdx38OMblpvcfwqzyJZL8Whfc-kWLUEY0nbpmpw8QaAxDRhAh6PYfA6OGPK_Gyc5hGde2vyIvkIiI

**Pod**: Kubernetes không chạy các container một cách trực tiếp, thay vào đó nó bọc một hoặc vài container vào với nhau trong một cấu trúc gọi là Pod. Pod là đơn vị nhỏ nhất của Kubernetes, các container cùng một pod thì chia sẻ với nhau tài nguyên và mạng cục bộ của pod…

.. figure:: https://2.bp.blogspot.com/-a7tbQ-93uCA/W9tl4zgHlVI/AAAAAAAACGE/762QwS7_iFMOodmtN-BOtdeHy_I2TQjpgCLcBGAs/s1600/Screen%2BShot%2B2018-11-01%2Bat%2B1.45.06%2BPM.png

**Replica Set**: ReplicaSet là loại resource dùng để tạo và duy trì số lượng Pod được chỉ định. Chẳng hạn như muốn tạo ra 5 Pod và muốn luôn duy trì số lượng này (nhằm đảm bảo ứng dụng hoạt động ổn định), trong trường hợp vì lý do gì đó, một hoặc một vào pod bị sự cố. K8s sẽ tự động tái tạo lại đủ 5 Pod.

**Deployment**: quản lý một nhóm các Pod - các Pod được nhân bản, nó tự động thay thế các Pod bị lỗi, không phản hồi bằng pod mới nó tạo ra. Như vậy, deployment đảm bảo ứng dụng của bạn có một (hay nhiều) Pod để phục vụ các yêu cầu. Deployment sử dụng mẫu Pod (Pod template - chứa định nghĩa / thiết lập về Pod) để tạo các Pod (các nhân bản replica), khi template này thay đổi, các Pod mới sẽ được tạo để thay thế pod cũ ngay lập tức.

**Service**: Các Pod được quản lý trong Kubernetes, trong vòng đời của nó chỉ diễn ra theo hướng - được tạo ra, chạy và khi nó kết thúc thì bị xóa và khởi tạo Pod mới thay thế. Có nghĩa ta không thể có tạm dừng Pod, chạy lại Pod đang dừng... Mặc dù mỗi Pod khi tạo ra nó có một IP để liên lạc, tuy nhiên vấn đề là mỗi khi Pod thay thế thì là một IP khác, nên các dịch vụ truy cập không biết IP mới nếu ta cấu hình nó truy cập đến Pod nào đó cố định. Để giải quyết vấn đề này sẽ cần đến Service. Service (microservice) là một đối tượng trừu tượng nó xác định ra một nhóm các Pod và chính sách để truy cập đến Pod đó. Nhóm các Pod mà Service xác định thường dùng kỹ thuật Selector (chọn các Pod thuộc về Service theo label của Pod).

