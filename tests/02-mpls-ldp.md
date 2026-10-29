# TC02 – Kiểm chứng MPLS LDP và chuyển tiếp nhãn

## Mục tiêu
Kiểm tra LDP neighbor và bảng MPLS forwarding (LFIB) trên mạng ISP.

## Thực hiện
```cisco
show mpls interfaces
show mpls ldp neighbor
show mpls forwarding-table
```
Trên CE-A1:
```cisco
traceroute 192.168.20.1 source Loopback10
```

## Kết quả đã quan sát
- PE1 có LDP neighbor Oper với P1 (`10.255.0.5`) và P2 (`10.255.0.6`) khi hai liên kết hoạt động.
- Các tuyến transport đến các PE đích có MPLS forwarding entry.
- Traceroute CE-A1 → CE-A2 xuất hiện MPLS label stack trên đường PE1–P1–PE2.

## Giới hạn
Chưa có file PCAP/Wireshark xác minh nhãn ở từng hop; nhãn có thể thay đổi giữa các lần khởi động.

**Trạng thái:** ĐẠT đối với LDP và MPLS forwarding đã ghi nhận.
