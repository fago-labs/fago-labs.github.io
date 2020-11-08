# ovs-dpctl: quản lý Open vSwitch datapath 

## cách sử dụng: 

```ovs-dpctl [options] command [switch] [args...]```

#### Command

  `add-dp DP [IFACE...]`   -  thêm mới datapath DP (with IFACEs)
  
  `del-dp DP`            -    xóa local datapath DP
  
  `add-if DP IFACE... `  -   thêm mỗi IFACE như là port trên DP
  
  `set-if DP IFACE...`    -   cấu hình lại mỗi IFACE trong DP
  
  `del-if DP IFACE... `   -   xóa từng IFACE từ DP
  
  `dump-dps`          -       hiện tên tất cả datapaths
  
  `show`             -        hiện thông tin cơ bản tất cả datapaths
  
  `show DP...`        -  hiện flows trong DP

  `add-flow [DP] FLOW ACTIONS` - thêm FLOW với ACTIONS vào DP
  
  `mod-flow [DP] FLOW ACTIONS` - thay đổi FLOW actions thành ACTIONS trong DP
  
  `get-flow [DP] ufid:UFID` -   fetch flow tương ứng với UFID
  
  `del-flow [DP] FLOW`     -    xóa FLOW từ DP
  
  `del-flows [DP]`        -     xóa tất cả flow từ DP
  
  `dump-conntrack [DP] [zone=ZONE]` - display conntrack entries for ZONE
  
  `flush-conntrack [DP] [zone=ZONE] [ct-tuple]` - delete matched conntrack entries in ZONE
  
  `ct-stats-show [DP] [zone=ZONE] [verbose]` - CT connections grouped by protocol
  
  `ct-bkts [DP] [gt=N]` - display connections per CT bucket
  
*Each IFACE on add-dp, add-if, and set-if may be followed by
comma-separated options.  See ovs-dpctl(8) for syntax, or the
Interface table in ovs-vswitchd.conf.db(5) for an options list.
For COMMAND dump-flows, add-flow, mod-flow, del-flow and
del-flows, DP is optional if there is only one datapath.*

#### Logging options:

  `-vSPEC`, `--verbose=SPEC` -  set logging levels
  
  `-v`, `--verbose`      -      set maximum verbosity level
  
  `--log-file[=FILE]`   -     enable logging to specified FILE      (default: /var/log/openvswitch/ovs-dpctl.log)
  
  `--syslog-method=(libc|unix:file|udp:ip:port)`  -    specify how to send messages to syslog daemon
  
  `--syslog-target=HOST:PORT` - also send syslog msgs to HOST:PORT via UDP
  

#### Options for show and mod-flow:

  `-s`,  `--statistics`     -     hiện số liệu thống kê cho port hoặc flow

#### Options for dump-flows:

  `-m`, `--more`           -       increase verbosity of output
  
  `--names`            -         use port names in output
  

#### Options for mod-flow:
  `--may-create`        -        tạo flow nếu chưa tồn tại
  
  `--read-only`        -         không chạy lệnh read/write 
  
  `--clear`            -        đặt lại số liệu thống kê hiện có về không
  

#### Other options:
  `-t`, `--timeout=SECS`    -      give up after SECS seconds
  
  `-h`, `--help`            -      display this help message
  
  `-V`, `--version`          -     display version information
