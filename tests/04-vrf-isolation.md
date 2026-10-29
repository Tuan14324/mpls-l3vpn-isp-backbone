# TC04 – Kiểm tra VRF và địa chỉ IP trùng nhau

## Mục tiêu
Xác minh Customer A và B dùng cùng các dải `192.168.10.0/24` và `192.168.20.0/24` mà có bảng định tuyến VRF tách biệt.

## Chính sách đã được đối chiếu
| PE | VRF | RD | RT import/export |
|---|---|---|---|
| PE1 | CUST-A | 65000:101 | 65000:100 |
| PE2 | CUST-A | 65000:102 | 65000:100 |
| PE3 | CUST-A | 65000:103 | 65000:100 |
| PE1 | CUST-B | 65000:201 | 65000:200 |
| PE4 | CUST-B | 65000:204 | 65000:200 |

## Lệnh kiểm tra
```cisco
show ip vrf detail
show ip route vrf CUST-A
show ip route vrf CUST-B
show running-config | section ip vrf
```

## Kết quả đã xác minh
Các VRF trên PE1–PE4 có RD riêng và RT theo thiết kế; bảng định tuyến PE1 tồn tại prefix trùng nhau trong hai VRF.

## Kiểm thử âm tính chưa hoàn thành
1. Quảng bá một prefix chỉ thuộc Customer A và không trùng prefix B.
2. Xác nhận prefix đó xuất hiện trong CUST-A nhưng không trong CUST-B ở các PE.
3. Thực hiện chiều ngược lại với prefix mới riêng Customer B.
4. Đối chiếu RT, route-map và kiểm tra không có route leaking cố ý/ngoài ý muốn.

**Trạng thái:** ĐẠT phần kiểm tra cấu hình và bảng tuyến; **CHƯA XÁC MINH ĐẦY ĐỦ** bài kiểm thử âm tính route leaking.
