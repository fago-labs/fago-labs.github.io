# `ovs-vsctl` : truy vấn và cấu hình ovs-vswitchd

## Cách sử dụng : 

       ```ovs-vsctl [options] -- [options] command [args] [-- [options] command [args]]...```




#### Open vSwitch commands:

  `init`                 -       khởi tạo database
  
  `show `              -       hiển thị tổng quan database content 
  
  `mer-reset `         -        reset cấu hình về clean state

#### Bridge commands:
  
 `add-br BRIDGE `        -     tạo bridge mới có tên BRIDGE
  
  `add-br BRIDGE PARENT VLAN`  - tạo mới fake BRIDGE in PARENT on VLAN
  
  `del-br BRIDGE `      -        xóa bridge tên BRIDGE và tất cả những port của nó
  
  `list-br `           -         liệt kê tên tất cả bridge
  
 `exists BRIDGE `    -    exit 2 nếu bridge tên BRIDGE không tồn tại
  
 `br-to-vlan BRIDGE`      -     hiển thị vlan nếu BRIDGE đang bật
  
  `br-to-parent BRIDGE`     -   hiện parent của BRIDGE
  
 `br-set-external-id BRIDGE KEY VALUE` - đặt KEY trên BRIDGE là VALUE
  
  
  `br-set-external-id BRIDGE KEY` - hủy cài đặt KEY trên BRIDGE
  
  `br-get-external-id BRIDGE KEY` - hiển thị giá trị KEY on BRIDGE
  
  `br-get-external-id BRIDGE` - liệt kê các cặp key-value trên BRIDGE
  
#### Port commands (a bond is considered to be a single port - một liên kết được coi là một cổng duy nhất):
  
 `list-ports BRIDGE`      -     liệt kê tên tất cả các port trên bridge có tên BRIDGE
  
 ` add-port BRIDGE PORT`  -      thêm network device PORT vào BRIDGE
  
 ` add-bond BRIDGE PORT IFACE...` - thêm bonded port PORT vào BRIDGE từ IFACES
  
  `del-port [BRIDGE] PORT`  -    xóa PORT (có thể đã được liên kết) của BRIDGE
  
  
  `port-to-br PORT`      -       hiện tên bridge chứa PORT

#### Interface commands (a bond consists of multiple interfaces - một liên kết bao gồm nhiều giao diện):
  
  `list-ifaces BRIDGE`    -    hiện tên tất cả interface trên BRIDGE
  
  `iface-to-br IFACE`     -      hiện tên bridge có chứa IFACE
  
#### Controller commands:
  
  `get-controller BRIDGE`  -    hiện controllers dùng cho BRIDGE
  
  `del-controller BRIDGE`  -    xóa controllers cho BRIDGE
  
  `set-controller BRIDGE TARGET...` -  thiết lập controllers cho BRIDGE
  
  `get-fail-mode BRIDGE`   -    hiện fail-mode cho BRIDGE
  
  `del-fail-mode BRIDGE`   -    xóa fail-mode cho BRIDGE
  
  `set-fail-mode BRIDGE MODE` - thiết lập fail-mode cho BRIDGE thành MODE
  
#### Manager commands:
  
  `get-manager`       -         hiện managers
  
  `del-manager`      -         xóa managers
  
 ` set-manager TARGET...`  -    thiết lập danh sách các managers từ TARGET...
  
#### SSL commands:
  
  `get-ssl `       -             hiện cấu hình cài đặt SSL 
  
 `del-ssl`            -         xóa cấu hình cài đặt SSL 
  
  `set-ssl PRIV-KEY CERT CA-CERT` - cài đặt cấu hình SSL 
  
#### Auto Attach commands:

  `add-aa-mapping BRIDGE I-SID VLAN ` -  add Auto Attach mapping to BRIDGE
  
  `del-aa-mapping BRIDGE I-SID VLAN` - delete Auto Attach mapping VLAN from BRIDGE
  
  `get-aa-mapping BRIDGE`         -     get Auto Attach mappings from BRIDGE



  
#### Switch commands:
  
  `emer-reset`         -         reset switch về trạng thái tốt nhất đã biết
  
#### Database commands:
  
  `list TBL [REC]`        -      liệt kê RECord (hoặc tất cả records) trong TBL
  
  `find TBL CONDITION...`   -    liệt kê records thỏa mãn CONDITION trong TBL
  
  `get TBL REC COL[:KEY]`   -    hiện giá trị cột của RECord trong TBL
  
 ` set TBL REC COL[:KEY]=VALUE` - đặt giá trị cột của RECord trong TBL
  
  `add TBL REC COL [KEY=]VALUE` - thêm (KEY=)VALUE vào cột của RECord trong TBL
  
  `remove TBL REC COL [KEY=]VALUE` -  loại bỏ (KEY=)VALUE từ cột
  
  `clear TBL REC COL `    -      dọn sạch giá trị cột của RECord trong TBL
  
  `create TBL COL[:KEY]=VALUE` - khởi tạo record mới
  
 ` destroy TBL REC`       -      xóa RECord từ TBL
  
  `wait-until TBL REC [COL[:KEY]=VALUE]` -  đợi đến khi điều kiện đúng
  
** Các lệnh cơ sở dữ liệu tiềm ẩn không an toàn yêu cầu tùy chọn --force. ** 

#### Options:

  `--db=DATABASE `     -         kết nối tới DATABASE                 (mặc định: unix:/ovs/var/run/openvswitch/db.sock)
  
  `--no-wait `         -         không đợi ovs-vswitchd cấu hình lại
  
  `--retry`          -           luôn cố gắng kết nối đến máy chỉ
  
  `-t, --timeout=SECS `   -      đợi tối đa SECS giây cho ovs-vswitchd
  
  `--dry-run `       -           không commit changes tới database
  
  
  `--oneline`        -           in chính xác một dòng đầu ra cho mỗi lệnh
  
  
#### Output formatting options:

  `-f`, `--format=FORMAT`    -     đặt định dạng output là FORMAT            ("table", "html", "csv", or "json")
                              
 ` -d`, `--data=FORMAT`     -      đặt định dạng table cell output là     FORMAT ("string", "bare", or "json")
                              
  `--no-headings`        -       bỏ qua hàng tiêu đề bảng
  
 ` --pretty `        -           pretty-print JSON ở output
  
  `--bare `          -           tương đương với "--format=list --data=bare --no-headings"

  
  
#### Logging options:
  
  `-vSPEC`, `--verbose=SPEC`  -  đặt logging levels
  
  `-v`, `--verbose`     -       đặt mức độ chi tiết tối đa
  
  `--log-file[=FILE]`  -      cho phép ghi log vào FILE               (mặc định: /ovs/var/log/openvswitch/ovs-vsctl.log)
  
  `--syslog-target=HOST:PORT` -  cũng gửi syslog msgs tới HOST:PORT qua UDP
  
  `--no-syslog `    -        tương đương với --verbose=vsctl:syslog:warn
  
#### Active database connection methods:

  `tcp:IP:PORT`      -       PORT at remote IP
  
  `ssl:IP:PORT`      -       SSL PORT at remote IP
  
 ` unix:FILE`         -      Unix domain socket named FILE
  
#### Passive database connection methods:

  `ptcp:PORT[:IP]`    -      listen to TCP PORT on IP
  
 ` pssl:PORT[:IP]`      -    listen for SSL on PORT on IP
  
  `punix:FILE `       -      listen on Unix domain socket FILE
  
#### PKI configuration (required to use SSL):

 ` -p`, `--private-key=FILE` - file with private key
  
  `-c`, `--certificate=FILE` - file with certificate for private key
  
  
 ` -C`, `--ca-cert=FILE `   -  file with peer CA certificate
  

#### SSL options:

  `--ssl-protocols=PROTOS` - danh sách SSL protocols để enable
  
  `--ssl-ciphers=CIPHERS` -  danh sách SSL ciphers để enable
  


#### Other options:

 ` -h`, `--help `       -          hiển thị hướng dẫn
  
 ` -V`, `--version `    -         hiển thị thông tin phiên bản
