# TC01 – Kiểm tra IS-IS Level 2 Backbone

## Mục tiêu
Xác minh adjacency IS-IS Level 2 và khả năng định tuyến giữa các Loopback0 của PE, P và RR.

## Mô hình
ISP AS 65000; PE1–PE4, P1, P2, RR1, RR2; IS-IS instance `CORE`, Level 2.

## Thực hiện
Trên PE1:
```cisco
show isis neighbors
show isis database
show ip route isis
ping 10.255.0.4 source Loopback0
```

## Kết quả ghi nhận
- PE1 thiết lập adjacency với P1 qua Ethernet0/3 và P2 qua Ethernet0/1.
- Cơ sở dữ liệu IS-IS thể hiện tám router PE/P/RR.
- PE1 ping Loopback0 của PE4 (`10.255.0.4`) thành công 5/5 trong bài kiểm tra đã ghi nhận.

**Trạng thái:** ĐẠT với các hạng mục được ghi nhận. Nên bổ sung ảnh CLI vào `evidence/isis/`.
