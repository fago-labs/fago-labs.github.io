Hướng dẫn sử dụng Acunetix
==========================

.. contents:: Nội dung

1. Yêu cầu phần cứng
-----------------------
-   Cấu hình: 2 CPU 4GB RAM
-   Hệ điều hành: Ubuntu, Kali Linux...
-   VMware Worstation 15.5
-   Kết nối mạng

2. Sử dụng Acunetix
-------------------

Giao diện web Acunetix
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Acunetix có giao diện web đơn giản, cho phép nhiều người dùng sử dụng Acunetix cùng lúc. 
Sau khi đăng nhập, người dùng sẽ được chuyển đến dashboard của Acunetix. Hiển thị thông tin tổng quan về hệ thống

.. figure:: https://user-images.githubusercontent.com/32956424/98456742-eb91de00-21b3-11eb-82b2-63551d4f5498.png

Người dùng có thể truy cập vào các tính năng khác, bao gồm:

-   **Target**: Thêm và cấu hình Target để quét. Theo dõi tình trạng bảo mật của Target bằng cách tổng hợp và hiển thị những lỗ hổng 
-   **Vulnerabilities**: Tất cả những lỗ hổng bảo mật được phát hiện sẽ được thống kê ở đây. Cho phép người dùng xác định và đánh giá mức độ ưu tiên của từng lỗ hổng. Ngoài ra còn có tính năng lọc theo nhiều tiêu chí, cho phép quá trình tìm kiếm dễ dàng hơn
-   **Scan**: Xem lại chi tiết kết quả của các quá trình scan, đã hoàn thành hoặc đang diễn ra 
-   **Report**: Tạo báo cáo chi tiết về quá trình scan Target



3. Cấu hình và scan Target
--------------------------------------

Tước khi quét, người dùng cần thêm Target. Target có thể là trang web hoặc ứng dụng web mà người dùng muốn quét.

Click **Target** và nhập địa chỉ của web muốn quét

.. figure:: https://user-images.githubusercontent.com/32956424/98457644-fcdee880-21bb-11eb-9028-30e161813904.png

Giao diện quét

.. figure:: https://user-images.githubusercontent.com/32956424/98458484-c1e0b300-21c3-11eb-819f-90c32560cec5.png


Cấu hình site login
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Trong nhiều  trường hợp, người dùng có thể chọn tính năng **Site Login** để tự động đăng nhập vào các ứng dụng web, bằng cách cung cấp Username và Password, Acunetix sẽ tự động xác định đường dẫn đăng nhập và đăng xuất của ứng dụng web, đồng thời giữ cho session luôn active. Việc sử dụng Site Login sẽ giúp Acunetix truy cập vào một số vùng bị hạn chế

.. figure:: https://user-images.githubusercontent.com/32956424/98458588-e8531e00-21c4-11eb-8c64-ac6033b98a70.png


Tính năng nâng cao khác
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Acunetix còn có nhiều tùy chọn nâng cao khác trước khi tiến hành quét, như:

-   Tùy chọn thu thập thông tin với Crawling Option
-   Loại bỏ đường dẫn khi quét một mục tiêu cụ thể
-   Xác thực HTTP
-   Chứng chỉ người dùng
-   Tùy chỉnh header
-   Tùy chỉnh cookies...


Tiến hành quét
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Từ danh sách **Target**, chọn Target muốn scan. Click vào phần **Scan**

.. figure:: https://user-images.githubusercontent.com/32956424/98458711-213fc280-21c6-11eb-8ee9-4657f0e9066a.png

Trong phần cấu hình Target, click **Scan**

.. figure:: https://user-images.githubusercontent.com/32956424/98458727-3d436400-21c6-11eb-85e2-a018269b38ac.png

Để đảm bảo là Target không còn những lỗ hổng bảo mật, tính năng **Continuous Scanning** cho phép quá trình scan Target diễn ra hàng ngày và phản hồi ngay lập tức nếu phát hiện lỗ hổng

Tùy chỉnh scan

.. figure:: https://user-images.githubusercontent.com/32956424/98458967-8c8a9400-21c8-11eb-8b87-d0264f91e441.png


-   **Scan Type**: Kiểu scan, có thể chọn Full Scan để quét toàn bộ lỗ hổng hoặc chọn 1 loại scan nhất định
-   **Report**: Tự động tạo báo cáo khi quá trình scan kết thúc
-   **Schedule**: Lập lịch để quét


Những loại scan đặc biệt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   **Full Scan**: Quét toàn bộ ứng dụng web sử dụng tất cả bộ kiểm tra có sẵn của Acunetix
-   **High Risk Vulnerabilities**: Chỉ quét những lỗ hổng có nguy cơ bảo mật cao
-   **Cross-Site Scripting (XSS)**: Quét lỗ hổng XSS
-   **SQL Injection**: Quét hỗ hổng SQL Injection
-   **Weak Passwords**: Xác định những form chấp nhận Username - Password và sẽ tấn công các form này
-   **Crawl Only**: Chỉ thu thập dữ liệu về trang web mà không tiến hành kiểm tra lỗ hổng nào

Quá trình scan phát hiện lỗ hổng XSS

.. figure:: https://user-images.githubusercontent.com/32956424/98459071-9234a980-21c9-11eb-98ac-ccc103166547.png



4. Xem xét kết quả Scan
--------------------------------------

Tại Dashboard, click vào phần **Scan** để hiển thị danh sách quét

.. figure:: https://user-images.githubusercontent.com/32956424/98459093-dde75300-21c9-11eb-959d-37d7c2402172.png

Khi quá trình scan kết thúc, Acunetix sẽ gửi cho người dùng 1 bản tổng hợp về quá trình scan kèm theo đường link trực tiếp đến kết quả scan đó. Kết quả scan hiển thị thời gian bắt đầu, kết thúc và thời lượng quá trình scan. Kèm theo là những cảnh báo đã xác định khi tiến hành quét 

Kết quả scan của từng Target bao gồm 4 phần:

-    **Scan Stats & Info**: Cung cấp cái nhìn tổng quan về quá trình quét Target. Chẳng hạn như thời gian quét, thời gian phản hồi trung bình, số lượng file đã quét  
-    **Vulnerabilities**: Danh sách những lỗ hổng bảo mật đã phát hiện trong quá trình quét
-    **Site Structure**: Hiển thị cấu trúc thư mục của web
-    **Events**: Danh sách các sự kiện liên quan đến quá trình scan, hiển thị thời gian bắt đầu và kết thúc scan, các lỗi phát sinh


 
5. Tạo báo cáo kết quả
--------------------------------------

Trong phần **Report**, người dùng có thể tùy chọn tạo ra 3 loại báo cáo khác nhau. 

.. figure:: https://user-images.githubusercontent.com/32956424/98459663-80560500-21cf-11eb-90fe-04d0fd97fd57.png

-    **All Vulnerabilities report**: Báo cáo toàn bộ các lỗ hổng đã phát hiện khi tiến hành scan Target
-    **Scan Report**: Báo cáo về những lỗ hổng được phát hiện bởi một hoặc nhiều lần scan
-    **Target Report**: Báo cáo về những lỗ hổng được phát hiện trên nhiều Target

Báo cáo cũng có thể được tùy chọn khi tiến hành scan Target

Khi tạo báo cáo, người dùng cần phải chọn template cho báo cáo. Báo cáo được tạo có thể ở định dạng PDF hoặc HTML cho phép người dùng download

.. figure:: https://user-images.githubusercontent.com/32956424/98459938-1e4acf00-21d2-11eb-9be3-70872f5a504f.png

Template báo cáo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-    **Affected Items Report**: Hiển thị các file và vị trí có lỗ hổng đã được phát hiện trong quá trình quét. Cho thấy mức độ nguy hiểm của lỗ hổng và các thông số chi tiết khác
-    **Developer Report**: Báo cáo này thường gửi đến các Developer, những người làm việc với ứng web để khắc phục những lỗ hổng do Acunetix phát hiện. Cung cấp thông tin về những file có thời gian phản hồi lâu, danh sách liên kết ngoài, script ở client... cùng với những đề xuất khắc phục
-    **Executive Report**: Tóm tắt các lỗ hổng được phát hiện trên web và đưa ra mức độ nghiêm trọng của các lỗ hổng được tìm thấy
-    **Quick Report**: Cung cấp danh sách chi tiết tất cả các lỗ hổng được phát hiện trong quá trình quét.
-    **Scan Comparison**: Cho phép so sánh 2 lần quét trên cùng 1 Target, và chỉ ra những điểm khác biệt giữa 2 lần quét đó. Người dùng chỉ có thể chọn template báo cáo này khi đã thực hiện scan 2 lần trên cùng 1 Target
-    **Compliance Reports**: Báo cáo có sẵn cho những cơ quan tuân thủ tiêu chuẩn

        + CWE / SANS – Top 25 Most Dangerous Software Errors
        + The Health Insurance Portability and Accountability Act (HIPAA)
        + International Standard – ISO 27001
        + NIST Special Publication 800-53
        + OWASP Top10 2017
        + Payment Card Industry (PCI) standards
        + Sarbanes Oxley Act
        + DISA STIG Web Security
        + Web Application Security Consortium (WASC) Threat Classification
            
