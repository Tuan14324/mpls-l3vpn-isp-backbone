# Xử lý sự cố thường gặp

## IS-IS không hình thành neighbor
- Kiểm tra IP/link: `show ip interface brief`.
- Kiểm tra cấu hình IS-IS và Level-2 trên cả hai đầu.
- Kiểm tra interface đã `no shutdown` và `isis network point-to-point` phù hợp.

## MPLS LDP không hình thành
- `show mpls interfaces`, `show mpls ldp neighbor`.
- Kiểm tra `mpls ip` trên cổng backbone và đường đến LDP router ID.

## VPNv4 không có prefix
- `show bgp vpnv4 unicast all summary`.
- Xác nhận MP-BGP neighbor tới RR, `send-community extended`, RD/RT và route import/export.
- Kiểm tra khả năng đến Loopback RR bằng underlay IS-IS.

## CE không ping được site khác
- Kiểm tra tuyến trong VRF trên PE, eBGP/OSPF PE–CE, nhãn MPLS và tuyến quay về.
- Với các khách hàng IP trùng nhau, kiểm tra **đúng VRF**, không kết luận từ ping chung chung.

## Quan sát khi RR1 lỗi
- BGP hold time expired có thể xảy ra trước khi trạng thái chuyển Idle; RR2 cần vẫn Established.
- RR không nằm trên đường dữ liệu customer; sự cố RR không nhất thiết gây mất gói.
