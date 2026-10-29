# TC03 – Kiểm chứng dịch vụ MPLS L3VPN đa khách hàng

## Mục tiêu
Đảm bảo các site thuộc cùng VPN có thể trao đổi tuyến và liên lạc qua backbone.

## Thiết kế
- CUST-A: CE-A1↔PE1, CE-A2↔PE2, CE-A3↔PE3; PE–CE dùng eBGP (AS 65111, 65112, 65113); RT 65000:100.
- CUST-B: CE-B1↔PE1 và CE-B2↔PE4; PE–CE dùng OSPF; RT 65000:200.

## Lệnh xác minh
Trên PE1:
```cisco
show bgp vpnv4 unicast all summary
show ip route vrf CUST-A
show ip route vrf CUST-B
```
Trên CE-A1:
```cisco
ping 192.168.20.1 source Loopback10
ping 192.168.30.1 source Loopback10
```
Trên CE-B1:
```cisco
show ip ospf neighbor
show ip route ospf
ping 192.168.20.1 source Loopback10
```

## Kết quả đã ghi nhận
- CE-A1 ping đến CE-A2 và CE-A3 thành công 5/5.
- CE-B1 ping CE-B2 thành công 5/5.
- Các bảng VRF PE1 có tuyến tương ứng với hai VPN, sử dụng next-hop PE2/PE3/PE4.

**Trạng thái:** ĐẠT đối với các bài kết nối đã ghi nhận.
