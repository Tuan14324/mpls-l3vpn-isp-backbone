# Bắt gói Wireshark (kế hoạch chưa thực hiện)

## Điểm bắt gói đề xuất
- PE1 E0/0: IPv4 ICMP Customer A (CE-A1 ↔ PE1).
- PE1 E0/3: MPLS PE1 ↔ P1.
- PE1 E0/1: MPLS PE1 ↔ P2 sau khi chuyển tuyến.
- P1 E1/1: MPLS P1 ↔ PE2.
- PE2 E0/0: IPv4 ICMP PE2 ↔ CE-A2.
- P2 E0/3: TCP/179 của RR2 qua P2.

## Display filters
```text
mpls
icmp
tcp.port == 179
tcp.port == 646
udp.port == 646
```

Các PCAP chưa được thu thập. Không ghi các kết quả suy đoán vào tài liệu kiểm thử.
