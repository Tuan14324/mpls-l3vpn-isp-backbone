# Danh mục xác minh kỹ thuật

| Nội dung | Lệnh kiểm tra | Bằng chứng |
|---|---|---|
| IS-IS | `show isis neighbors`, `show ip route isis` | [TC01](../tests/01-isis-backbone.md) |
| MPLS LDP | `show mpls ldp neighbor`, `show mpls forwarding-table` | [TC02](../tests/02-mpls-ldp.md) |
| VPNv4 | `show bgp vpnv4 unicast all summary` | [TC03](../tests/03-mpls-l3vpn.md) |
| VRF | `show ip vrf detail`, `show ip route vrf CUST-A` | [TC04](../tests/04-vrf-isolation.md) |
| Link failover | IS-IS neighbor, LDP và LFIB trước/sau shutdown | [TC05](../tests/05-link-failover.md) |
| RR failover | RR1 Idle, RR2 Established, CE-A2 học tuyến mới | [TC06](../tests/06-rr-failover.md) |
| Performance | Packet loss, RTT, recovery time | [TC07](../tests/07-performance.md) |

**Nguyên tắc:** chỉ gắn PASS cho những kết quả đã được lưu/ghi nhận. Ghi rõ giới hạn phương pháp đo.
