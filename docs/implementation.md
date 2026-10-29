# Quy trình triển khai lab

1. Dựng đúng topology 13 router theo ảnh tại `topology/network-topology.png`.
2. Thiết lập interface IPv4 /30 và Loopback0 /32 theo [bảng IP](../topology/ip-addressing.md).
3. Cấu hình IS-IS `CORE` Level 2 trên các liên kết provider và quảng bá Loopback0.
4. Bật MPLS LDP trên các liên kết PE–P, P–P; dùng Loopback0 làm LDP router ID.
5. Tạo VRF `CUST-A` và `CUST-B` trên các PE tham gia; cấu hình RD, RT và gán cổng CE vào VRF.
6. Cấu hình RR1/RR2 phản xạ VPNv4 và kết nối PE1–PE4 bằng địa chỉ Loopback0, AS 65000.
7. Cấu hình eBGP PE–CE cho Customer A, OSPF PE–CE cho Customer B.
8. Đối chiếu cấu hình thực tế trong `configs/` rồi chạy các bài test ở `tests/`.

Không sao chép trực tiếp cấu hình vào thiết bị khác nếu cổng hoặc IOS image khác. Không phân phối image Cisco IOL trong repository.
