# TC06 – Dự phòng MP-BGP VPNv4 Route Reflector

## Mục tiêu
Kiểm chứng RR2 tiếp tục phân phối tuyến VPNv4 mới khi RR1 bị tắt.

## Điều kiện
Backbone IS-IS/MPLS hoạt động, PE1–PE4 kết nối VPNv4 tới RR1/RR2; các site Customer A liên lạc được.

## Kịch bản
1. Stop RR1 trên EVE-NG; giữ RR2 và các router lõi hoạt động.
2. Trên PE1 xác nhận RR1 Down, RR2 Established.
3. Tạo Loopback20 `192.168.110.1/24` trên CE-A1 và quảng bá mạng mới qua eBGP.
4. Kiểm tra RR2 nhận VPNv4 prefix và CE-A2 học được tuyến mới.
5. Ping/traceroute từ CE-A2 đến Loopback20 CE-A1.

### Lệnh kiểm tra PE1
```cisco
show bgp vpnv4 unicast all summary
show ip route 10.255.0.7
ping 10.255.0.7 source Loopback0
```

### Kết quả thực tế PE1
- RR1 `10.255.0.7`: BGP **Idle**, route đến Loopback RR1 không còn.
- RR2 `10.255.0.8`: **Established**, nhận 4 prefix ở thời điểm kiểm tra.
- Ping RR1: 0/5.

### Kết quả thực tế RR2
```text
Route Distinguisher: 65000:101
*>i 192.168.110.0  10.255.0.1  0 100 0 65111 i
```

### Kết quả thực tế CE-A2
```text
B 192.168.110.0/24 [20/0] via 172.16.12.1
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/3 ms
```
Traceroute:
```text
1 172.16.12.1
2 10.0.25.2 [MPLS: Labels 20/35 Exp 0]
3 172.16.11.1 [MPLS: Label 35 Exp 0]
4 172.16.11.2
```

## Kết luận
**PASS:** RR2 vẫn nhận và phân phối tuyến VPNv4 mới đến CE-A2 khi RR1 Down, và lưu lượng đến CE-A1 thông suốt trong lần ping được ghi nhận. Không đồng nhất kết quả ping 5 gói với số liệu packet loss trong khoảnh khắc xảy ra lỗi.

## Dọn dẹp thử nghiệm
Sau khi bật lại RR1, nếu muốn xóa prefix thử:
```cisco
configure terminal
router bgp 65111
 address-family ipv4
  no network 192.168.110.0 mask 255.255.255.0
 exit-address-family
no interface Loopback20
end
```
