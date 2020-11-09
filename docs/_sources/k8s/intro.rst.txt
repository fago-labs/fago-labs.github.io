Giới thiệu về Kubernetes
==========================================

.. contents:: Nội dung

1. Đặt vấn đề
-----------------------

Hiện nay, trong lĩnh vực công nghệ phần mềm, các kiến trúc bắt đầu thay đổi từ Monolithic sang Microservice, từ cồng kềnh to lớn sang chia nhỏ, nhẹ nhàng linh hoạt hơn, nhằm đáp ứng nhu cầu phát triển rất nhanh của các công ty và doanh nghiệp.

Bài toán đặt ra ở đây là:

- Chuẩn thức hóa ứng dụng, nghĩa là ứng dụng có thể chạy ở mọi nơi mà không phụ thuộc nhiều vào môi trường
- Làm thế nào để triển khai ứng dụng nhanh chóng
- Làm thế nào để scale, đáp ứng tải
- Làm thế nào để quản lý các tài nguyên (CPU, RAM...) hiệu quả

.. figure:: https://lh3.googleusercontent.com/jgauUsfU7rwTwNYS4qnyMA9jS0UGVkdDGbjYsZ6Th9rfDIz6PDczHJET8jALP-hUhMigU4leJeMcw_mnrPGwakgTyWnBaaIlvWHuTqcRzewH4MCRKl2nWDDpZ6Jlld8Xf1jyuWU

Trước khi nói về Kubernetes, cùng nhìn lại quá trình triển khai ứng dụng qua các thời kỳ:


- Thời đại triển khai theo cách truyền thống: Ban đầu, các ứng dụng được chạy trên máy chủ vật lý, các ứng dụng này cùng sử dụng tài nguyên của máy chủ, đôi khi dẫn đến sự cố phân bổ tài nguyên. Ví dụ, nếu nhiều ứng dụng cùng chạy trên một máy chủ vật lý, có thể có những trường hợp một ứng dụng sẽ chiếm phần lớn tài nguyên hơn và kết quả là các ứng dụng khác sẽ hoạt động kém đi. Giải pháp ở đây là chạy 1 ứng dụng trên một máy chủ vật lý, tuy nhiên cách này gây lãng phí tài nguyên và sẽ rất tốn kém

- Thời đại triển khai ảo hóa: Sau này, ảo hóa đã được giới thiệu như một giải pháp thay thế, Nó cho phép chạy nhiều Máy ảo (VM) trên một máy chủ vật lý. Ảo hóa cho phép các ứng dụng được cô lập giữa các VM và cung cấp mức độ bảo mật, vì thông tin của một ứng dụng không thể được truy cập tự do bởi một ứng dụng khác. Ảo hóa cho phép sử dụng tốt hơn các tài nguyên trong một máy chủ vật lý và cho phép khả năng mở rộng tốt hơn

- Thời đại triển khai Container: Các container tương tự như VM, nhưng chúng có tính cô lập để chia sẻ Hệ điều hành (HĐH) giữa các ứng dụng. Container nhẹ hơn VM, container có hệ thống tệp (filesystem), CPU, bộ nhớ, process space…

Container đã trở nên phổ biến vì chúng có thêm nhiều lợi ích như: sử dụng tài nguyên hiệu quả, độc lập nền tảng, mở rộng nhanh chóng, hoạt động đơn giản, cải thiện năng suất...

2. Kubernetes là gì
-------------------------------

Kubernetes (thường được gọi tắt là k8s) là một nền tảng nguồn mở, khả chuyển, có thể mở rộng để quản lý các ứng dụng được đóng gói và các service, giúp thuận lợi trong việc cấu hình và tự động hóa việc triển khai ứng dụng.

Phát triển bởi Google và ra mắt vào năm 2014, Kubernetes là một phần trong hệ sinh thái container ngày nay, đóng vai trò là là một công cụ orchestration ( tạo ra, điều phối và chỉ huy các container )

Là một hệ thống toàn diện để tự động hóa việc triển khai, lập lịch và nhân rộng các ứng dụng được đóng gói, hỗ trợ nhiều công cụ container hóa như Docker.

.. figure:: https://lh4.googleusercontent.com/4uM2zfK93y8KzJJ4Xyj3oZhD1492C9EUKHqJxyOcDOnybjXS4dI45duRJsPw1DIH06nb0CIELB9oqo6JgY-LHsPoEDGeTGehfhn2wJID-t1XX5sKYYh2ElG3Lufd7m8Y490PLME

3. Ứng dụng của Kubernetes
-------------------------------

Kubernetes đảm nhiệm việc nhân rộng và chuyển đổi dự phòng cho ứng dụng, cung cấp các mẫu deployment.
Kubernetes còn cung cấp:

- Server discovery và cân bằng tải: Kubernetes có thể expose một container sử dụng DNS hoặc địa chỉ IP của riêng nó. Nếu lượng traffic truy cập đến một container cao, Kubernetes có thể cân bằng tải và phân phối lưu lượng mạng (network traffic) để việc triển khai được ổn định.

- Điều phối bộ nhớ: Kubernetes cho phép tự động mount một hệ thống lưu trữ mà bạn chọn, như local storages, public cloud providers...

- Tự động rollouts và rollbacks: Tự động hoá Kubernetes để tạo mới các container cho việc triển khai, xoá các container hiện có và áp dụng tất cả các resource của chúng vào container mới.

- Đóng gói tự động: Cung cấp cho Kubernetes một cluster bao gồm node dùng để chạy các tác vụ được đóng gói. Người quản trị có thể định nghĩa mỗi container cần vào nhiêu CPU và RAM. Kubernetes có thể điều phối các container đến các node để tận dụng tốt nhất các resource  

- Tự phục hồi: Kubernetes khởi động lại các containers bị lỗi, thay thế các container, xoá các container không phản hồi lại cấu hình health check

- Quản lý cấu hình và bảo mật: Kubernetes cho phép lưu trữ và quản lý các thông tin nhạy cảm như: password, OAuth token và SSH key. Có thể triển khai và cập nhật lại secret và cấu hình ứng dụng mà không cần build lại các container image và không để lộ secret

