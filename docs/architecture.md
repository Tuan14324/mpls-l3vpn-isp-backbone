# Kiến trúc hệ thống MPLS L3VPN

## Các lớp mạng
- **PE1–PE4:** tiếp nhận các kết nối CE, duy trì VRF, trao đổi VPNv4 và thực hiện MPLS encapsulation/decapsulation.
- **P1/P2:** mạng lõi, chuyển tiếp nhãn vận chuyển; các PE có kết nối tới cả hai P.
- **RR1/RR2:** phản xạ tuyến VPNv4, không tham gia chuyển tiếp dữ liệu khách hàng.
- **CE-A1/A2/A3:** Customer A dùng eBGP, ba AS 65111/65112/65113.
- **CE-B1/B2:** Customer B dùng OSPF PE–CE.

## Giao thức
IS-IS Level 2 cho underlay IP, LDP cho nhãn transport, MP-BGP VPNv4 cho tuyến/nhãn VPN. CUST-A dùng RT 65000:100 và CUST-B dùng RT 65000:200. RD khác nhau trên các PE giúp phân biệt VPNv4 NLRI.

## Khả năng dự phòng
PE nối đến P1 và P2; mỗi PE có phiên VPNv4 đến hai RR. RR1 nối qua P1, RR2 nối qua P2. Khi RR1 Down, phiên RR2 vẫn hỗ trợ quảng bá VPNv4 mới (đã được kiểm thử).

## Tài liệu liên quan
- [Bảng IP](../topology/ip-addressing.md)
- [Kiểm thử RR](../tests/06-rr-failover.md)
- [Kiểm thử chuyển tuyến](../tests/05-link-failover.md)
