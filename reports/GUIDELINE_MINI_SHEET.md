# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Lê Sĩ Thành<br>
**MSSV:** 2A202602125<br>
**Hình thức:** SOLO<br>
**Mã cặp:** `SOLO`

## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| ---: | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: `drive_008` / vật thể nghi bus
- Dấu hiệu nhìn thấy: Xe có thân dài, kích thước lớn và nhiều cửa sổ dọc thân.
- Quy tắc áp dụng: Xe khách thân dài, nhiều cửa sổ hoặc hàng ghế được gán `bus`; van là xe thân hộp nhỏ hơn.
- Quyết định: `bus`
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Đặt `review_state = needs_review`, không đoán và xin reviewer/Lab Coach kiểm tra.

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: `drive_033` / vật thể nghi truck
- Dấu hiệu nhìn thấy: Xe có phần khoang hàng/thùng tách biệt rõ với cabin.
- Quy tắc áp dụng: Xe có thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng được gán `truck`.
- Quyết định: `truck`
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Đặt `review_state = needs_review` và xin reviewer kiểm tra trước khi quyết định.

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: `drive_033` / vật thể bị xe khác che một phần
- Dấu hiệu nhìn thấy khi phóng 100%: Vẫn nhìn thấy đủ đặc điểm để xác định lớp nhưng một phần thân xe bị phương tiện khác che.
- Giá trị `visibility`: `occluded`
- Giá trị `boundary`: `inside`
- Trạng thái `review_state`: `confident`
- Lý do: Vật thể bị phương tiện khác che nhưng không bị mép ảnh cắt và vẫn có đủ bằng chứng để xác định lớp.

## 6. Xác nhận tự kiểm tra

- [x] Đã rà đủ bốn ảnh.
- [x] Đã kiểm vật thể thiếu và trùng.
- [x] Đã kiểm lớp và hình học từng hộp.
- [x] Mỗi hộp có đủ ba thuộc tính.
- [x] Đã xử lý mọi hộp `needs_review`.
- [x] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [x] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [x] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
- [x] Số vật thể thực tế: — 40–60 là mục tiêu khối lượng, không phải điểm cắt.
