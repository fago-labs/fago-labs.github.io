# ovsdb-tool: quản lý Open vSwitch database 

## cách sử dụng: 

#### Command

```ovsdb-tool [OPTIONS] COMMAND [ARG...]```


  `create [DB [SCHEMA]]` -  tạo DB với SCHEMA đã cho
  
  `create-cluster DB CONTENTS LOCAL`      -     tạo clustered DB với CONTENTS  và LOCAL address đã cho
  
 ` [--cid=UUID] join-cluster DB NAME LOCAL REMOTE...` - tham gia clustered DB  với NAME và LOCAL và REMOTE addrs đã cho
 
  `compact [DB [DST]]`  -    compact DB in-place (or to DST)
  
  `convert [DB [SCHEMA [DST]]]` -  convert DB to SCHEMA (to DST)
  
  `db-name [DB]`       -     báo cáo tên của schema dùng bởi DB
  
  `db-version [DB]`   -     báo cáo phiên bản của schema dùng bởi DB
  
  `db-cksum [DB]`     -     báo cáo checksum của schema dùng bởi DB
  
  `db-cid DB`        -      báo cáo cluster ID của clustered DB
  
  `db-sid DB`        -       báo cáo server ID của clustered DB
  
  `db-local-address DB `  -  báo cáo local address của clustered DB
  
  `db-is-clustered DB`   -  kiểm tra xem DB có clustered (phân cụm)
  
  `db-is-standalone DB`   -  kiểm tra xem DB có phải standalone
  
  `schema-name [SCHEMA]`  -  báo cáo tên của SCHEMA
  
  `schema-version [SCHEMA]` - báo cáo phiên bản schema của SCHEMA
  
  `schema-cksum [SCHEMA]` - báo cáo checksum của SCHEMA
  
  `compare-versions A OP B` - so sánh số phiên bản OVSDB schema 
  
  `query [DB] TRNS`    -    thực hiện read-only transaction trên DB
  
  `transact [DB] TRNS`   -   thực hiện read/write transaction trên DB
  
  `[-m]... show-log [DB]` -  hiện log của DB
  
*The default DB is /etc/openvswitch/conf.db.*

*The default SCHEMA is /usr/share/openvswitch/vswitch.ovsschema.*

#### Logging options:

  `-vSPEC`, `--verbose=SPEC` -  set logging levels
  
  `-v`, `--verbose`      -      set maximum verbosity level
  
  `--log-file[=FILE]`   -     enable logging to specified FILE     (default: /var/log/openvswitch/ovsdb-tool.log)
  
  `--syslog-method=(libc|unix:file|udp:ip:port)` -     specify how to send messages to syslog daemon
  
  `--syslog-target=HOST:PORT` - also send syslog msgs to HOST:PORT via UDP

#### Other options:

  `-m`, `--more`          -        increase show-log verbosity
  
  `--rbac-role=ROLE `      -     RBAC role for transact and query commands
  
  `-h`, `--help `         -       display this help message
  
  `-V`, `--version`        -       display version information
