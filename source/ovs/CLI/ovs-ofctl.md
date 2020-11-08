# ovs-ofctl: quản lý OpenFlow switch 

## cách sử dụng: 

```ovs-ofctl [options] command [switch] [args...]```


#### For OpenFlow switches:

  `show SWITCH`           -      hiện thông tin  OpenFlow của SWITCH (bridge hoặc socket)
  
  `dump-desc SWITCH`       -    hiện description của SWITCH
  
  `dump-tables SWITCH`     -     hiện số liệu trong bảng của SWITCH
  
  `dump-table-features SWITCH` - hiện đặc tính của bảng của SWITCH
  
  `mod-port SWITCH IFACE ACT` -  sửa đổi behavior của port
  
  `mod-table SWITCH MOD`     -   sửa đổi behavior của flow table 
  
  `get-frags SWITCH`       -     hiện hành vi xử lý phân mảnh 
  
  `set-frags SWITCH FRAG_MODE` - thiết lập hành vi xử lý phân mảnh
  
  `dump-ports SWITCH [PORT]` -  hiện thông tin PORT của SWITCH 
  
  `dump-ports-desc SWITCH [PORT]` - hiện thông tin  PORT của SWITCH
  
  `dump-flows SWITCH`      -     hiện tất cả flow entries của SWITCH
  
  `dump-flows SWITCH FLOW`   -  hiện matching FLOWs của SWITCH
  
  `dump-aggregate SWITCH `    -  hiện thống kê aggregate flow của SWITCH
  
  `dump-aggregate SWITCH FLOW` -  hiện aggregate stats for FLOWs
  
  `queue-stats SWITCH [PORT [QUEUE]]` -  dump queue stats
  
  `add-flow SWITCH FLOW`     -   thêm flow được mô tả bởi FLOW vào SWITCH
  
  `add-flows SWITCH FILE`    -   thêm flows từ FILE vào SWITCH
  
  `mod-flows SWITCH FLOW`    -   sửa đổi hành động matching flow 
  
  `del-flows SWITCH [FLOW]`   -  xóa matching FLOWs
  
  `replace-flows SWITCH FILE`  -  thay thế flow bằng các flow trong FILE
  
  `diff-flows SOURCE1 SOURCE2` - so sánh flow từ hai nguồn SOURCE1 và SOURCE2
  
  `packet-out SWITCH IN_PORT ACTIONS PACKET...`   -   thực thi ACTIONS trên PACKET
  
  `monitor SWITCH [MISSLEN] [invalid_ttl] [watch:[...]]` -    hiện packets đã nhận từ SWITCH
  
  `snoop SWITCH`              -  snoop on SWITCH and its controller
  
  `add-group SWITCH GROUP`    -  thêm group được mô tả bởi GROUP vào SWITCH
  
  `add-groups SWITCH FILE`   -   thêm group từ FILE
  
  `mod-group SWITCH GROUP`    -  sửa đổi group tên GROUP của SWITCH
  
  `del-groups SWITCH [GROUP]` -  xóa GROUP
  
  `dump-group-features SWITCH` -  hiện thuộc tính các group trên SWITCH
  
  `dump-groups SWITCH [GROUP]` - hiện mô tả các group trên SWITCH
  
  `dump-group-stats SWITCH [GROUP]` - hiện số liệu thống kê của GROUP
  
  `queue-get-config SWITCH PORT` - hiện thông tin hàng đợi của port
  
  `add-meter SWITCH METER`   -   thêm meter mô tả bởi METER vào SWITCH
  
  `mod-meter SWITCH METER`   -   sửa đổi METER
  
  
  `del-meter SWITCH METER`  -    xóa METER
  
  `del-meters SWITCH`      -     xóa tất cả meter của SWITCH
  
  `dump-meter SWITCH METER`  -  hiện cấu hình METER 
  
  `dump-meters SWITCH`      -    hiện cấu hình tất cả metet
  
 ` meter-stats SWITCH [METER]`-  hiện thống kê METER
 
  `meter-features SWITCH`   -    hiện thuộc tính meter
  
#### For OpenFlow switches and controllers:

  `probe TARGET`         -       thăm dò xem TARGET có up không 
  
  `ping TARGET [N]`      -       ping xem độ trễ bằng N-byte 
  
  
  `benchmark TARGET N COUNT` -   bandwidth of COUNT N-byte echos
  
*SWITCH or TARGET is an active OpenFlow connection method.*

#### Other commands:
  `ofp-parse FILE `     -        hiện thông điệp đọc được từ FILE
  
  `mod-temp-thresh SWITCH THRESHOLD` -  modify temperature threshold
  
  `dump-temp-thresh SWITCH` -    print temperature threshold
  
  
  `ofp-parse-pcap PCAP`     -    hiện OpenFlow đọc từ PCAP
  
  `dump-tables-desc SWITCH` -    hiện mô tả bảng
  
  `bundle SWITCH MSG`      -     gửi bundle messages
  
#### Active OpenFlow connection methods:

  `tcp:IP[:PORT]`     -      PORT (default: 6633) at remote IP
  
  `ssl:IP[:PORT]`      -     SSL PORT (default: 6633) at remote IP
  
  `unix:FILE`         -      Unix domain socket named FILE
  
  
#### PKI configuration (required to use SSL):

  `-p`, `--private-key=FILE` -  file with private key
  
  `-c`, `--certificate=FILE` - file with certificate for private key
  
  `-C`, `--ca-cert=FILE`   -   file with peer CA certificate
  
#### Daemon options:

  `--detach`        -        run in background as daemon
  
  `--no-chdir`       -       do not chdir to '/'
  
  `--pidfile[=FILE]`    -    create pidfile (default: /ovs/var/run/openvswitch/ovs-ofctl.pid)
  
  `--overwrite-pidfile`  -   with --pidfile, start even if already running
  
#### OpenFlow version options:

  `-V`, `--version`      -     hiện thông tin phiên bản
  
  `-O`, `--protocols`    -     cho phép phiên bản OpenFlow nào      (default: OpenFlow10, OpenFlow11, OpenFlow12, OpenFlow13, OpenFlow14)
  
#### Logging options:

  `-vSPEC`, `--verbose=SPEC` -  đặt logging levels
  
  `-v`, `--verbose`       -     đặt maximum verbosity level
  
  `--log-file[=FILE]`     -   cho phép ghi log vào FILE        (default: /ovs/var/log/openvswitch/ovs-ofctl.log)
  
  `--syslog-target=HOST:PORT` -  đồng thời gửi syslog msgs tới HOST:PORT qua UDP
  
#### Other options:

  `--strict`           -         use strict match for flow commands
  
  `--readd`             -        thay thế các flow không thay đổi
  
  `-F`, `--flow-format=FORMAT`  -  ép định dạng flow
  
  `-P`, `--packet-in-format=FRMT` - ép định dạng packet
  
  `-m`, `--more`         -         nhiều thông tin chi tiết hơn
  
  `--timestamp`          -       (monitor, snoop) hiện timestamps
  
  `-t`, `--timeout=SECS`     -     loại bỏ sau SECS giây
  
  `--sort[=field]`          -     sắp xếp theo thứ tự tăng dần
  
  `--rsort[=field]`         -     sắp xếp theo thứ tự giảm dần
  
  `--unixctl=SOCKET`       -     đặt tên control socket
  
  `-h`, `--help`           -       hiện thông tin hỗ trợ
  
  `-V`, `--version`        -       hiện thông tin phiên bản
