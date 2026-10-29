# TC05 – Dự phòng liên kết PE1–P1

## Mục tiêu
Kiểm chứng IS-IS và MPLS chuyển tuyến từ PE1–P1 sang PE1–P2 khi liên kết chính bị ngắt.

## Tạo sự cố
Trên PE1:
```cisco
configure terminal
interface Ethernet0/3
 shutdown
end
```

## Kiểm tra
```cisco
show isis neighbors
show mpls ldp neighbor
show ip route 10.255.0.2
show mpls forwarding-table
```

## Kết quả thực tế đã ghi nhận
- LDP neighbor P1 (`10.255.0.5`) Down và IS-IS neighbor P1 không còn trong bảng.
- IS-IS và LDP với P2 (`10.255.0.6`) duy trì UP/Oper.
- Tuyến `10.255.0.2/32` đến PE2 đi qua `10.0.16.2` trên Ethernet0/1, IS-IS metric 20.
- LFIB cho `10.255.0.2/32` dùng outgoing label 22 qua P2; các tuyến PE3/PE4 cũng sử dụng PE1 Ethernet0/1.

## Phục hồi
```cisco
configure terminal
interface Ethernet0/3
 no shutdown
end
```

**Trạng thái:** ĐẠT về định tuyến và MPLS forwarding qua đường dự phòng. Chưa ghi nhận số liệu packet loss hoặc convergence time bằng timestamp chính xác.
