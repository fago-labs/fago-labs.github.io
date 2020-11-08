# ovsdb-client: Open vSwitch database JSON-RPC client
## cách sử dụng: 
```ovsdb-client [OPTIONS] COMMAND [ARG...]```

#### Valid commands are:

  `list-dbs [SERVER]` -   liệt kê databases có sẵn trên SERVER

  `get-schema [SERVER] [DATABASE]` -  truy xuất lược đồ cho DATABASE từ SERVER

  `get-schema-version [SERVER] [DATABASE]` -  truy xuất lược đồ cho DATABASE từ SERVER và chỉ báo cáo  số phiên bản trên stdout

  `get-schema-cksum [SERVER] [DATABASE]` - truy xuất lược đồ cho DATABASE từ SERVER và chỉ báo cáo   tổng kiểm tra trên stdout

 ` list-tables [SERVER] [DATABASE]` - liệt kê các bảng cho DATABASE trên SERVER

  `list-columns [SERVER] [DATABASE] [TABLE]` - liệt kê các cột trong TABLE (hoặc tất cả các bảng) trong DATABASE trên SERVER
     
  `transact [SERVER] TRANSACTION` -  chạy TRANSACTION (thông số cho yêu cầu "transact") trên SERVER   và in kết quả dưới dạng JSON trên stdout

  `query [SERVER] TRANSACTION` - chạy TRANSACTION (thông số cho yêu cầu "transact") trên SERVER,  dưới dạng chỉ đọc và in kết quả dưới dạng JSON trên stdout

 ` monitor [SERVER] [DATABASE] TABLE [COLUMN,...]...` - theo dõi nội dung của COLUMN trong TABLE trong DATABASE trên SERVER. COLUMN có thể bao gồm !initial, !insert, !delete, !modify để tránh nhìn thấy the specified kinds of changes.

 ` monitor-cond [SERVER] [DATABASE] CONDITION TABLE [COLUMN,...]...` -  mtheo dõi nội dung khớp với CONDITION của COLUMN trong TABLE trong DATABASE trên SERVER. COLUMN có thể bao gồm !initial, !insert, !delete, !modify để tránh nhìn thấy the specified kinds of changes.

  `convert [SERVER] SCHEMA` - convert database on SERVER named in SCHEMA to SCHEMA.

 ` needs-conversion [SERVER] SCHEMA` - kiểm tra xem db của SCHEMA trên SERVER có cần conversion hay không

  `monitor [SERVER] [DATABASE] ALL` - giám sát tất cả các thay đổi đối với tất cả các cột trong tất cả các bảng

  `wait [SERVER] DATABASE STATE` - đợi đến khi DATABASE đạt đến STATE ("added" or "connected" or "removed") trong DATBASE trên SERVER.

  `dump [SERVER] [DATABASE]` - dump contents of DATABASE on SERVER to stdout

  `backup [SERVER] [DATABASE] > SNAPSHOT` - dump database contents in the form of a database file

  `[--force] restore [SERVER] [DATABASE] < SNAPSHOT` - khôi phục nội dung cơ sở dữ liệu từ một file cơ sở dữ liệu


  `lock [SERVER] LOCK` - tạo hoặc chờ LOCK trong SERVER

  `steal [SERVER] LOCK` - lấy LOCK từ SERVER

  `unlock [SERVER] LOCK` - unlock LOCK t SERVER

*The default SERVER is unix:/var/run/openvswitch/db.sock.*

*The default DATABASE is Open_vSwitch.*

#### Active SERVER connection methods:

  `tcp:IP:PORT`       -     PORT at remote IP
  
  `ssl:IP:PORT`       -      SSL PORT at remote IP
  
  `unix:FILE`         -      Unix domain socket named FILE
  
#### Passive SERVER connection methods:

  `ptcp:PORT[:IP]`     -     listen to TCP PORT on IP
  
  `pssl:PORT[:IP]`     -     listen for SSL on PORT on IP
  
  `punix:FILE`         -     listen on Unix domain socket FILE
  
#### PKI configuration (required to use SSL):

  `-p`, `--private-key=FILE` - file with private key
  
  `-c`, `--certificate=FILE` - file with certificate for private key
  
  `-C`, `--ca-cert=FILE`    -  file with peer CA certificate
  
  `--bootstrap-ca-cert=FILE` - file with peer CA certificate to read or create
  
#### SSL options:

  `--ssl-protocols=PROTOS` -  list of SSL protocols to enable
  
  `--ssl-ciphers=CIPHERS` -  list of SSL ciphers to enableget-schema [SERVER] [DATABASE]
   
#### Output formatting options:

  `-f`, `--format=FORMAT`    -     set output formatting to FORMAT    ("table", "html", "csv", or "json")
  
  `-d`, `--data=FORMAT`     -      set table cell output formatting to  FORMAT ("string", "bare", or "json")
  
  `--no-headings`      -         omit table heading row
  
  `--pretty`          -          pretty-print JSON in output
  
  `--bare`             -         equivalent to "--format=list --data=bare --no-headings"
  
  `--timestamp`        -         timestamp "monitor" output
  
#### Daemon options:

#### Output formatting options:

  `-f`, `--format=FORMAT`    -     set output formatting to FORMAT       ("table", "html", "csv", or "json")
  
  `-d`, `--data=FORMAT`     -      set table cell output formatting to   FORMAT ("string", "bare", or "json")
  
  `--no-headings`        -       omit table heading row
  
  `--pretty`            -        pretty-print JSON in output
  
  `--bare`              -        equivalent to "--format=list --data=bare --no-headings"
  
  `--timestamp`         -        timestamp "monitor" output
  
#### Daemon options:

  `--detach`        -        run in background as daemon
  
  `--monitor`      -         creates a process to monitor this daemon
  
  `--user=username[:group]` - changes the effective daemon user:group
  
  `--no-chdir`         -     do not chdir to '/'
  
  `--pidfile[=FILE]`    -    create pidfile (default: /var/run/openvswitch/ovsdb-client.pid)
  
  `--overwrite-pidfile `  -  with --pidfile, start even if already running
  

#### Logging options:

  `-vSPEC`, `--verbose=SPEC` -  set logging levels
  
  `-v`, `--verbose`       -     set maximum verbosity level
  
  `--log-file[=FILE] `   -    enable logging to specified FILE  (default: /var/log/openvswitch/ovsdb-client.log)
  
  `--syslog-method=(libc|unix:file|udp:ip:port)` -  specify how to send messages to syslog daemon
  
  `--syslog-target=HOST:PORT` - also send syslog msgs to HOST:PORT via UDP

#### Other options:

  `-h`, `--help`         -         display this help message
  
  `-V`, `--version`       -        display version information
