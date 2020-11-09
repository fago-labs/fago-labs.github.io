
Open vSwitch
===========

switch 24 port
================


*Mục tiêu*

- Tạo một switch 24 port có các chức năng cơ bản của switch layer 2 và chứa năng định tuyến tĩnh dựa vào địa chỉ ip của layer 3

- Các port 1 - 20 làm access port cho các VLAN, port 21 - 24 làm trunk port


**Tạo switch có tên switch1**

1. Tạo biến
--------------

::

    bridge_name=switch1


2. Tạo bridge switch1 ở fail-secure mode để Open Flow table rỗng khi khởi tạo
------------------

::

    ovs-vsctl add-br bridge_name -- set Bridge bridge_name fail-mode=secure


3. Tạo các port cho swtich với tùy chọn ofport_request (cần tạo các port trên máy trước)
-----------------------

::

    for i in  1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 ; do
        ovs-vsctl add-port switch1 ens$i  -- set Interface ens$i ofport_request=$i
        ovs-ofctl mod-port switch1 ens$i up
    done    

4. Bật giao thức STP
-----------------

::

    ovs-vsctl set bridge $bridge_name stp_enable=true


5. Thêm các flow table vào switch
----------------

::

    # table 0
    ovs-ofctl add-flow $bridge_name "table=0, priority=0, actions=resubmit(,3)"

    # table 3 - static forwarding and filter
    ovs-ofctl add-flow $bridge_name "table=3, priority=0, actions=resubmit(,5)"

    # table 5 - VLAN
    ovs-ofctl add-flows $bridge_name - <<'EOF'
    table=5, priority=99, in_port=1, vlan_tci=0, actions=mod_vlan_vid:10, resubmit(,10)
    table=5, priority=99, in_port=2, vlan_tci=0, actions=mod_vlan_vid:10, resubmit(,10)
    table=5, priority=99, in_port=3, vlan_tci=0, actions=mod_vlan_vid:10, resubmit(,10)
    table=5, priority=99, in_port=4, vlan_tci=0, actions=mod_vlan_vid:10, resubmit(,10)
    table=5, priority=99, in_port=5, vlan_tci=0, actions=mod_vlan_vid:10, resubmit(,10)
    table=5, priority=99, in_port=6, vlan_tci=0, actions=mod_vlan_vid:20, resubmit(,10)
    table=5, priority=99, in_port=7, vlan_tci=0, actions=mod_vlan_vid:20, resubmit(,10)
    table=5, priority=99, in_port=8, vlan_tci=0, actions=mod_vlan_vid:20, resubmit(,10)
    table=5, priority=99, in_port=9, vlan_tci=0, actions=mod_vlan_vid:20, resubmit(,10)
    table=5, priority=99, in_port=10, vlan_tci=0, actions=mod_vlan_vid:20, resubmit(,10)
    table=5, priority=99, in_port=11, vlan_tci=0, actions=mod_vlan_vid:30, resubmit(,10)
    table=5, priority=99, in_port=12, vlan_tci=0, actions=mod_vlan_vid:30, resubmit(,10)
    table=5, priority=99, in_port=13, vlan_tci=0, actions=mod_vlan_vid:30, resubmit(,10)
    table=5, priority=99, in_port=14, vlan_tci=0, actions=mod_vlan_vid:30, resubmit(,10)
    table=5, priority=99, in_port=15, vlan_tci=0, actions=mod_vlan_vid:30, resubmit(,10)
    table=5, priority=99, in_port=16, vlan_tci=0, actions=mod_vlan_vid:40, resubmit(,10)
    table=5, priority=99, in_port=17, vlan_tci=0, actions=mod_vlan_vid:40, resubmit(,10)
    table=5, priority=99, in_port=18, vlan_tci=0, actions=mod_vlan_vid:40, resubmit(,10)
    table=5, priority=99, in_port=19, vlan_tci=0, actions=mod_vlan_vid:40, resubmit(,10)
    table=5, priority=99, in_port=20, vlan_tci=0, actions=mod_vlan_vid:40, resubmit(,10)
    table=5, priority=99, in_port=21, vlan_tci=0, actions=mod_vlan_vid:20, resubmit(,10)
    table=5, priority=99, in_port=22, vlan_tci=0, actions=mod_vlan_vid:20, resubmit(,10)
    table=5, priority=99, in_port=23, vlan_tci=0, actions=mod_vlan_vid:30, resubmit(,10)
    table=5, priority=99, in_port=24, vlan_tci=0, actions=mod_vlan_vid:30, resubmit(,10)
    EOF
    ovs-ofctl add-flow $bridge_name "table=5, priority=0, actions=resubmit(,10)"

    # table 10 - learning
    ovs-ofctl add-flow $bridge_name \
        "table=10 actions=learn(table=25, NXM_OF_VLAN_TCI[0..11], \
                               NXM_OF_ETH_DST[]=NXM_OF_ETH_SRC[], \
                               load:NXM_OF_IN_PORT[]->NXM_NX_REG0[0..15]), \
                         resubmit(,15)"

    # table 15 - static forwarding
    ovs-ofctl add-flow $bridge_name "table=15, priority=0, actions=resubmit(,20)"

    # table 20 - look
    ovs-ofctl add-flow $bridge_name \
        "table=20 priority=50 actions=resubmit(,25), resubmit(,30)"

    # table 30 - forwarding
    # trunk port 21,22,23,24 
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=0 actions=21,22,23,24,strip_vlan,1,2,3,4,5,6,7 8 9 10 11 12 13 14 15 16 17 18 19 20"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=1 actions=strip_vlan,1"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=2 actions=strip_vlan,2"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=3 actions=strip_vlan,3"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=4 actions=strip_vlan,4"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=5 actions=strip_vlan,5"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=6 actions=strip_vlan,6"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=7 actions=strip_vlan,7"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=8 actions=strip_vlan,8"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=9 actions=strip_vlan,9"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=10 actions=strip_vlan,10"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=11 actions=strip_vlan,11"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=12 actions=strip_vlan,12"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=13 actions=strip_vlan,13"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=14 actions=strip_vlan,14"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=15 actions=strip_vlan,15"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=16 actions=strip_vlan,16"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=17 actions=strip_vlan,17"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=18 actions=strip_vlan,18"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=19 actions=strip_vlan,19"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=20 actions=strip_vlan,20"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=21 actions=21"
    Lab/protocol/

    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=22 actions=22"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=23 actions=23"
    ovs-ofctl add-flow $bridge_name "table=30 priority=50 reg0=24 actions=24"
    ovs-ofctl add-flow $bridge_name "table=30, priority=0, actions=drop"

**Trong đó**

- Table 0 là nơi gói tin đi qua đầu tiên

- Table 3 định tuyến tĩnh theo ip hoặc lọc 

- Table 5 thêm vlan_id theo port, ở đây coi các in_port 1->5 thuộc vlan1, 6->10 thuộc vlan2, 11->15 thuộc vlan3, 16->20 thuộc vlan4. 21->24 dùng làm port trunk hoặc nối với router

- Table 10 học địa chỉ MAC và VLAN từ các flow và lưu vào table 25

- Table 15 định tuyến tĩnh (chưa dùng đến)

- Table 20 có nhiệm vụ matching flow bằng table 25 dựa trên địa chỉ MAC đích và VLAN, sau đó chuyển tiếp sang table 30

- Table 30 chính thức forward gói tin. Đối với output ra các access port, ta thực hiện loại bỏ VLAN header trước đi đẩy gói tin ra. Còn trunk port thì vẫn giữ VLAN header


*Tạo tương tự các switch khác chỉ cần đổi switch_name.*


