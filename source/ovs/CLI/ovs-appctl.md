# `ovs-appctl` - truy vấn và điều khiển Open vSwitch daemon

## cách sử dụng: 

```ovs-appctl [--target=target | -t target] command [arg...]```

#### Targets:

  `-t`, `--target=TARGET` - pidfile or socket to contact
  
#### Common commands:

  `help`          -     những lệnh hỗ trợ với mục tiêu
  
 `version`        -    hiện phiên bản của mục tiêu
  
  `vlog/list`     -     Liệt kê các cấp độ ghi nhật ký hiện tại
  
  `vlog/set [SPEC]` - Đặt các mức nhật ký như được nêu chi tiết trong SPEC, có thể bao gồm:
      
  
   A valid module name (all modules, by default)
      
   'syslog', 'console', 'file' (all facilities, by default))
      
   'off', 'emer', 'err', 'warn', 'info', or 'dbg' ('dbg', bydefault)    
      
  `vlog/reopen`  -      tạo chương trình mở lại tệp nhật ký của nó
  
#### Other options:

  `--timeout=SECS`  -   đợi tốt đa SECS giây cho mỗi reponse
  
  `-h`, `--help`    -     hiện thông tin hỗ trợ có ích
  
  `-V`, `--version`   -  hiển thị thông tin phiên bản ovs-appctl 

