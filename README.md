 DỰ ÁN MÔ PHỎNG MẠNG ISP MPLS L3VPN TRÊN EVE-NG



Thiết kế, triển khai và kiểm thử hệ thống mạng ISP Backbone sử dụng IS-IS, MPLS LDP, MP-BGP VPNv4 và Route Reflector dự phòng.\*\*



 1. Giới thiệu dự án



Dự án xây dựng một mô hình mạng nhà cung cấp dịch vụ Internet (ISP) sử dụng công nghệ \*\*Multiprotocol Label Switching – Layer 3 Virtual Private Network (MPLS L3VPN)\*\* để cung cấp kết nối mạng riêng ảo cho nhiều khách hàng doanh nghiệp trên cùng một hệ thống mạng lõi.



Mô hình được triển khai bằng phần mềm EVE-NG, sử dụng 13 router Cisco IOL với các vai trò gồm Provider Edge (PE), Provider (P), Route Reflector (RR) và Customer Edge (CE).



Hệ thống sử dụng IS-IS Level 2 làm giao thức định tuyến nội bộ mạng ISP, MPLS LDP để phân phối nhãn vận chuyển và MP-BGP VPNv4 để trao đổi thông tin định tuyến VPN giữa các router PE.



Hai khách hàng doanh nghiệp được triển khai độc lập. Customer A sử dụng eBGP, trong khi Customer B sử dụng OSPF để trao đổi thông tin định tuyến với nhà cung cấp dịch vụ.



Dự án đồng thời triển khai hai Route Reflector nhằm dự phòng mặt phẳng điều khiển BGP và các liên kết backbone dự phòng nhằm duy trì khả năng kết nối khi một đường truyền gặp sự cố.



 2. Mục tiêu dự án



\- Thiết kế topology mạng ISP Backbone với các kết nối dự phòng.

\- Cấu hình IS-IS Level 2 bảo đảm khả năng định tuyến giữa các router lõi.

\- Triển khai MPLS LDP để xây dựng hệ thống chuyển tiếp dựa trên nhãn.

\- Cấu hình MP-BGP VPNv4 với hai Route Reflector.

\- Triển khai dịch vụ MPLS L3VPN cho hai khách hàng doanh nghiệp.

\- Sử dụng VRF, Route Distinguisher và Route Target để phân tách thông tin định tuyến.

\- Hỗ trợ các khách hàng sử dụng những dải địa chỉ IP trùng nhau.

\- Sử dụng eBGP và OSPF làm các giao thức định tuyến PE–CE.

\- Kiểm tra khả năng kết nối giữa các chi nhánh.

\- Thử nghiệm khả năng dự phòng khi liên kết backbone hoặc Route Reflector gặp sự cố.

\- Tổng hợp cấu hình, bằng chứng kiểm thử và tài liệu phục vụ GitHub Portfolio.



3. Môi trường triển khai



| Thành phần | Thông tin |

|-------------|------------|

| Phần mềm mô phỏng | EVE-NG |

| Thiết bị mạng | Cisco IOL |

| Phiên bản IOS | 15.7(3)M2 |

| Tổng số router | 13 |

| ISP Autonomous System | 65000 |

| Giao thức IGP | IS-IS Level 2 |

| MPLS | LDP |

| VPN Control Plane | MP-BGP VPNv4 |

| Khách hàng A | eBGP |

| Khách hàng B | OSPF |



\## 4. Thiết kế topology



!\[Sơ đồ mạng MPLS L3VPN](topology/network-topology.png)





\ 4.1. Thành phần thiết bị



| Nhóm thiết bị | Tên router | Số lượng | Vai trò |

|---|---|---:|---|

| Provider Edge | PE1, PE2, PE3, PE4 | 4 | Cung cấp dịch vụ VPN cho khách hàng |

| Provider Core | P1, P2 | 2 | Chuyển tiếp lưu lượng MPLS |

| Route Reflector | RR1, RR2 | 2 | Phân phối thông tin VPNv4 |

| Customer A | CE-A1, CE-A2, CE-A3 | 3 | Các chi nhánh khách hàng A |

| Customer B | CE-B1, CE-B2 | 2 | Các chi nhánh khách hàng B |

| \*\*Tổng cộng\*\* | | \*\*13\*\* | |



\ 4.2. Nguyên lý hoạt động



Mạng ISP sử dụng các router P để vận chuyển lưu lượng giữa những router PE. Các router PE thực hiện định tuyến theo từng VRF và đóng gói nhãn MPLS để chuyển dữ liệu qua mạng lõi.



IS-IS Level 2 bảo đảm khả năng định tuyến giữa các địa chỉ Loopback của thiết bị ISP.



MPLS LDP phân phối nhãn vận chuyển, còn MP-BGP VPNv4 trao đổi thông tin tuyến VPN và các nhãn VPN liên quan.



RR1 và RR2 thực hiện phản xạ các tuyến MP-BGP VPNv4. Chúng không phải thiết bị bắt buộc phải nằm trên đường đi của dữ liệu khách hàng.



Các chi nhánh thuộc cùng một khách hàng có thể liên lạc qua MPLS Backbone, trong khi thông tin định tuyến của hai khách hàng được duy trì trong các VRF khác nhau.



\ 5. Thiết kế địa chỉ IP



\ 5.1. Địa chỉ Loopback của ISP



| Router | Loopback0  |

|--------|------------|

| PE1 | 10.255.0.1/32 |

| PE2 | 10.255.0.2/32 |

| PE3 | 10.255.0.3/32 |

| PE4 | 10.255.0.4/32 |

| P1  | 10.255.0.5/32 |

| P2  | 10.255.0.6/32 |

| RR1 | 10.255.0.7/32 |

| RR2 | 10.255.0.8/32 |



\### 5.2. Mạng khách hàng



| Router| Mạng LAN        | Kết nối ISP|

|-------|-----------------|------------|

| CE-A1 | 192.168.10.0/24 | PE1 |

| CE-A2 | 192.168.20.0/24 | PE2 |

| CE-A3 | 192.168.30.0/24 | PE3 |

| CE-B1 | 192.168.10.0/24 | PE1 |

| CE-B2 | 192.168.20.0/24 | PE4 |



Các mạng LAN được mô phỏng thông qua giao diện Loopback10 trên những router CE.



Bảng địa chỉ đầy đủ của từng liên kết được trình bày trong tài liệu `topology/ip-addressing.md`.



\6. Thiết kế MPLS L3VPN



\ 6.1. Customer A



Customer A gồm ba chi nhánh sử dụng eBGP kết nối với mạng ISP.



| Router CE | ASN | PE |

|---|---|---|

| CE-A1 | 65111 | PE1 |

| CE-A2 | 65112 | PE2 |

| CE-A3 | 65113 | PE3 |



Thông số VPN:



\- Tên VRF: `CUST-A`

\- RT Import: `65000:100`

\- RT Export: `65000:100`



\ 6.2. Customer B



Customer B gồm hai chi nhánh sử dụng OSPF kết nối với mạng ISP.



Thông số VPN:



\- Tên VRF: `CUST-B`

\- RT Import: `65000:200`

\- RT Export: `65000:200`



\ 6.3. Phân bổ RD và RT



| Router PE | VRF | RD | RT Import/Export |

|---|---|---|---|

| PE1 | CUST-A | 65000:101 | 65000:100 |

| PE2 | CUST-A | 65000:102 | 65000:100 |

| PE3 | CUST-A | 65000:103 | 65000:100 |

| PE1 | CUST-B | 65000:201 | 65000:200 |

| PE4 | CUST-B | 65000:204 | 65000:200 |



Các router PE thuộc cùng VPN sử dụng Route Target tương ứng để import/export các tuyến VPN.



RD giúp phân biệt các tuyến VPNv4 có cùng IPv4 prefix, còn VRF và chính sách Route Target bảo đảm thông tin định tuyến được phân phối theo đúng khách hàng.



\ 6.4. Hỗ trợ địa chỉ IP trùng nhau



Customer A và Customer B sử dụng trùng hai mạng:



\- `192.168.10.0/24`

\- `192.168.20.0/24`



Dù địa chỉ IP trùng nhau, mỗi khách hàng vẫn sử dụng bảng định tuyến VRF riêng.



Đây là một chức năng quan trọng của MPLS L3VPN, cho phép nhà cung cấp phục vụ nhiều doanh nghiệp mà không bắt buộc các khách hàng phải sử dụng dải địa chỉ riêng biệt trên toàn bộ hệ thống ISP.



\ 7. Quy trình triển khai



Dự án được triển khai qua các giai đoạn:



\*\*Giai đoạn 1 – Cấu hình hạ tầng mạng\*\*



Thiết lập giao diện vật lý, địa chỉ IPv4 và Loopback0 trên toàn bộ router.



\*\*Giai đoạn 2 – Triển khai IS-IS Level 2\*\*



Cấu hình IS-IS trên các router PE, P và RR để cung cấp khả năng định tuyến nội bộ ISP.



\*\*Giai đoạn 3 – Triển khai MPLS LDP\*\*



Kích hoạt MPLS trên những liên kết backbone và kiểm tra quá trình thiết lập LDP neighbor, phân phối nhãn và forwarding.



\*\*Giai đoạn 4 – Triển khai MPLS L3VPN\*\*



Tạo VRF, RD và RT. Gán interface PE–CE vào VRF tương ứng.



\*\*Giai đoạn 5 – Cấu hình MP-BGP VPNv4\*\*



Thiết lập phiên MP-BGP giữa các router PE với RR1 và RR2 để phân phối các tuyến VPN.



\*\*Giai đoạn 6 – Cấu hình PE–CE Routing\*\*



Triển khai eBGP cho Customer A và OSPF cho Customer B.



\*\*Giai đoạn 7 – Kiểm tra kết nối\*\*



Kiểm tra bảng định tuyến khách hàng, ICMP, traceroute và chuyển tiếp MPLS.



\*\*Giai đoạn 8 – Kiểm thử dự phòng\*\*



Mô phỏng sự cố liên kết PE1–P1 và sự cố RR1, quan sát quá trình duy trì hoặc khôi phục dịch vụ.



\ 8. Kết quả kiểm thử



\ 8.1. IS-IS Backbone



Kết quả kiểm tra cho thấy các router ISP thiết lập adjacency IS-IS Level 2 và học được các tuyến backbone.



Các lệnh xác minh:



```cisco

show isis neighbors

show isis database

show ip route isis

```



\ 8.2. MPLS LDP



Các router PE duy trì LDP neighbor với những router P trực tiếp kết nối.



Các nhãn MPLS được thể hiện trong bảng forwarding và traceroute của lưu lượng khách hàng.



```cisco

show mpls ldp neighbor

show mpls forwarding-table

```



\ 8.3. MP-BGP VPNv4



PE1, PE2, PE3 và PE4 thiết lập phiên MP-BGP VPNv4 với RR1/RR2.



Bảng định tuyến VPNv4 thể hiện các prefix thuộc Customer A và Customer B với RD tương ứng.



```cisco

show bgp vpnv4 unicast all summary

show bgp vpnv4 unicast all

show ip route vrf CUST-A

show ip route vrf CUST-B

```



\ 8.4. Kết nối giữa các chi nhánh



Các kết quả ICMP đã được ghi nhận:



| Nguồn | Đích  |     Kết quả    |

|-------|-------|----------------|

| CE-A1 | CE-A2 | Thành công 5/5 |

| CE-A1 | CE-A3 | Thành công 5/5 |

| CE-B1 | CE-B2 | Thành công 5/5 |



Traceroute xác nhận lưu lượng Customer A đi qua hệ thống MPLS Backbone với các nhãn MPLS.



\ 9. Kiểm thử dự phòng



\ 9.1. Mất liên kết PE1–P1



Thao tác thực hiện:



```cisco

configure terminal

interface Ethernet0/3

&#x20;shutdown

end

```



Kết quả quan sát:



\- Adjacency IS-IS giữa PE1 và P1 bị mất.

\- LDP neighbor PE1–P1 chuyển trạng thái Down.

\- PE1 vẫn duy trì IS-IS và LDP với P2.

\- Tuyến đến PE2 được chuyển sang next-hop `10.0.16.2`.

\- Bảng MPLS forwarding trên PE1 cập nhật các nhãn để chuyển tiếp qua P2.



\*\*Kết luận:\*\* Cơ chế chuyển tuyến của IS-IS và MPLS đã được xác minh khi một liên kết backbone bị ngắt.



Chưa có số liệu định lượng về packet loss và traffic recovery time.



\ 9.2. Mất Route Reflector RR1



Thao tác: Stop RR1 trong EVE-NG, giữ RR2 và backbone hoạt động.



Kết quả:



\- Phiên BGP VPNv4 từ PE1 đến RR1 chuyển sang Idle.

\- PE1 không còn tuyến đến Loopback0 của RR1.

\- RR2 vẫn duy trì phiên MP-BGP VPNv4.

\- Một tuyến mới `192.168.110.0/24` được quảng bá từ CE-A1.

\- RR2 nhận được tuyến VPNv4 mới thuộc RD `65000:101`.

\- CE-A2 học được tuyến mới qua BGP.

\- CE-A2 ping đến `192.168.110.1` thành công 5/5 gói.



Kết quả ICMP:



```text

Success rate is 100 percent (5/5)

round-trip min/avg/max = 1/1/3 ms

```



Traceroute ghi nhận nhãn MPLS trên đường đi giữa các site.



\*\*Kết luận: PASS\*\*



RR2 tiếp tục thực hiện chức năng phân phối VPNv4 khi RR1 bị lỗi, đồng thời kết nối Customer A vẫn hoạt động.



\ 10. Trạng thái hoàn thành



| Hạng mục | Trạng thái |

|---|---|

| Thiết kế topology 13 router | Hoàn thành |

| IS-IS Level 2 | Đã kiểm chứng |

| MPLS LDP | Đã kiểm chứng |

| MP-BGP VPNv4 | Đã kiểm chứng |

| MPLS L3VPN Customer A | Đã kiểm chứng |

| MPLS L3VPN Customer B | Đã kiểm chứng |

| RD/RT PE1–PE4 | Đã kiểm chứng |

| Định tuyến với IP trùng nhau | Đã kiểm chứng |

| Chuyển tuyến PE1–P1 | Đã kiểm chứng control plane và bảng MPLS |

| RR1/RR2 Failover | PASS |

| Kiểm thử âm tính route leaking | Chưa có bằng chứng đầy đủ |

| Đo hiệu năng và thời gian hội tụ | Chưa thực hiện |

| Phân tích gói bằng Wireshark | Chưa thực hiện |



\ 11. Hạn chế và hướng phát triển



\- Hoàn thiện kiểm thử không rò rỉ tuyến giữa hai VRF.

\- Đo tỷ lệ mất gói và RTT khi liên kết backbone gặp sự cố.

\- Đánh giá traffic recovery time với công cụ có timestamp phù hợp.

\- Bắt gói và phân tích MPLS label stack bằng Wireshark.

\- Nghiên cứu tối ưu hóa phát hiện lỗi bằng BFD khi thiết bị hỗ trợ.

\- Xây dựng chính sách lọc tuyến và redistribution chặt chẽ hơn.

\- Nghiên cứu tự động hóa việc xác minh cấu hình và kết nối.



\ 12. Kiến thức và kỹ năng đạt được



Dự án giúp thực hành các kỹ năng:



\- Thiết kế hệ thống mạng ISP Backbone.

\- Cấu hình và xử lý sự cố IS-IS.

\- Triển khai MPLS LDP và phân tích nhãn MPLS.

\- Cấu hình MP-BGP VPNv4 và Route Reflector.

\- Triển khai MPLS L3VPN đa khách hàng.

\- Quản lý VRF, RD và Route Target.

\- Kết hợp eBGP và OSPF trong mạng nhà cung cấp dịch vụ.

\- Kiểm thử dự phòng mạng lõi.

\- Phân tích bảng định tuyến, bảng nhãn và traceroute.

\- Triển khai và quản lý mô hình mạng trên EVE-NG.



\13. Ghi chú



Dự án được xây dựng phục vụ nghiên cứu, học tập và phát triển kỹ năng Network Engineering.



Các file cấu hình được sử dụng trong môi trường mô phỏng. Không phân phối Cisco IOL image hoặc các phần mềm thương mại có bản quyền trong repository.

