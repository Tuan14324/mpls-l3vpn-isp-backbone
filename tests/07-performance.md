# TC07 – Đo hiệu năng và thời gian khôi phục (chưa thực hiện)

## Mục tiêu
Đo tỷ lệ mất gói (packet loss), RTT và thời gian khôi phục lưu lượng dưới các kịch bản lỗi.

## Kịch bản dự kiến
1. Baseline: CE-A1 → CE-A2 khi backbone bình thường.
2. Shutdown PE1–P1 khi ICMP đang chạy.
3. Tắt P1 và quan sát dự phòng qua P2.
4. Tắt RR1, xác minh RR2 duy trì chức năng điều khiển.

## Lệnh đo sơ bộ
```cisco
ping 192.168.20.1 repeat 1000 timeout 1 source Loopback10
```

## Biểu mẫu số liệu
| Kịch bản | Số gói gửi | Số gói nhận | Packet loss | RTT min/avg/max | Recovery time |
|---|---:|---:|---:|---|---|
| Baseline | — | — | Chưa đo | Chưa đo | N/A |
| PE1–P1 down | — | — | Chưa đo | Chưa đo | Chưa đo |
| P1 down | — | — | Chưa đo | Chưa đo | Chưa đo |
| RR1 down | — | — | Chưa đo | Chưa đo | Chưa đo |

Công thức: Packet loss (%) = (gói gửi – gói nhận) / gói gửi × 100.

**Lưu ý:** Ping Cisco IOS không đảm bảo khoảng cách gửi gói cố định; số gói mất không trực tiếp suy ra thời gian hội tụ theo ms. Để có độ chính xác cao hơn, nên dùng công cụ phát luồng có timestamp và phân tích PCAP.

**Trạng thái:** CHƯA ĐO; không được ghi số liệu giả định là kết quả thực nghiệm.
