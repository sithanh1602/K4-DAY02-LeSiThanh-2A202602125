# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Lê Sĩ Thành<br>
**MSSV:** 2A202602125<br>
**Hình thức:** SOLO<br>
**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp:f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33
- Bốn mã ảnh: drive_008, drive_022, drive_033, drive_038
- Số vật thể thực tế: 126
- Mã SHA-256 của gói YOLO của bạn: 823b4ce7887f8135d70dcd9386c497f14a7698cf0ed5e11efb0fa45e4ad0b06e
- Mã SHA-256 của gói CVAT gốc của bạn: f1f7f6c15bde3f6ec373d15decdf04dba890907d6fed099736dd9f37b6dd1c86
- Nguồn đối chiếu: Bộ tham chiếu do người hướng dẫn thực hành cấp
- Mã SHA-256 của gói đối chiếu: c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu:day2-reference-4img-v1
Thời điểm nhận: 14/09/2026

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:

Bài được thực hiện độc lập bằng cách tự gán nhãn và xuất dữ liệu từ CVAT trước khi sử dụng bộ dữ liệu đối chiếu. Bộ đối chiếu chỉ được sử dụng sau đó để kiểm tra, phát hiện sai lệch và đánh giá chất lượng kết quả gán nhãn.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| ----------- | ---- | ------------------ | --------------- |
| drive_033 / xe bị che một phần | car | Nhìn thấy hình dạng chính của xe nhưng một phần bị vật thể khác che khuất | Gán lớp car và đặt visibility = occluded |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

Lớp cho biết vật thể là gì, ví dụ `car`. Thuộc tính mô tả trạng thái hoặc đặc điểm của vật thể đó, ví dụ `visibility = occluded`. Vì vậy hai xe đều có lớp `car` nhưng có thể có thuộc tính visibility khác nhau như `clear` và `occluded`.

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| ------------- | -------- | -------------- | ---------------------- |
| visibility = `occured` | thuộc tính | Notebook audit CVAT-native báo giá trị `occured` không hợp lệ | Sửa thành `visibility = occluded`; thuộc tính phải dùng đúng các giá trị được quy định trong guideline |

- Số hộp `needs_review` trước và sau khi kiểm:
Cần lấy từ kết quả audit/notebook, chưa đủ dữ liệu để xác định chính xác.

- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ:
Khi một vật thể bị che khuất hoặc hình ảnh không đủ rõ để xác định chắc chắn lớp/thuộc tính, tôi đánh dấu `needs_review` và nhờ reviewer/người hướng dẫn kiểm tra thay vì tự suy đoán.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`:
- Tên lớp và tọa độ điểm ảnh `xyxy`:
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?

- Dòng `class x_center y_center width height`:
`0 0.494125 0.483734 0.126125 0.153156`

- Tên lớp và tọa độ điểm ảnh `xyxy`:
Lớp: `car`
`xyxy ≈ [275.88, 260.58, 356.60, 358.60]`

- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?

Một dòng YOLO có thể đúng cú pháp nhưng nội dung gán nhãn vẫn sai. Ví dụ, annotator có thể chọn nhầm lớp `car` thay vì `van`, vẽ box quá rộng hoặc quá hẹp, hoặc bỏ sót một phần của vật thể. Vì vậy cần kiểm tra cả định dạng và chất lượng annotation.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện:
- Mã ảnh thẩm định:
- Mô tả một dự đoán trong `detect_result.jpg`:
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
- Minh chứng nào có thể bác bỏ nhận định của bạn?
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?

- Ba mã ảnh huấn luyện:
`drive_022`, `drive_033`, `drive_038`

- Mã ảnh thẩm định:
`drive_008`

- Mô tả một dự đoán trong `detect_result.jpg`:
Mô hình được huấn luyện trên 3 ảnh và thẩm định trên `drive_008`. Tuy nhiên kết quả validation cho thấy mô hình chưa phát hiện đúng vật thể nào trên ảnh thẩm định, với Precision = 0, Recall = 0, mAP50 = 0 và mAP50-95 = 0.

- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
Kết quả này gợi ý cần kiểm tra lại chất lượng và tính nhất quán của dữ liệu gán nhãn, đặc biệt là lớp `car`, `truck`, `bus`, `van`, bounding box và các thuộc tính. Ngoài ra cần kiểm tra xem số lượng box có bị dư, thiếu hoặc sai lớp giữa các ảnh hay không.

- Minh chứng nào có thể bác bỏ nhận định của bạn?
Có thể đối chiếu annotation của `drive_008` với bộ tham chiếu do người hướng dẫn cấp. Nếu ground truth/reference cho thấy các class và bounding box trong dữ liệu đã đúng và nhất quán, thì kết quả kém có thể do tập huấn luyện chỉ có 3 ảnh và 8 epoch, chứ chưa đủ để kết luận dữ liệu bị lỗi.

- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?
Vì tập dữ liệu chỉ có 4 ảnh, trong đó 3 ảnh dùng để train và 1 ảnh dùng để validation. Kích thước dữ liệu quá nhỏ, không đủ đại diện cho các tình huống thực tế về góc nhìn, ánh sáng, mức độ che khuất, kích thước và sự đa dạng của phương tiện. Vì vậy kết quả này chỉ dùng để chẩn đoán và phản hồi chất lượng dữ liệu, không phải benchmark để triển khai mô hình thực tế.

## 6. Đối chiếu nhãn

- Số hộp ghép được:
- IoU trung bình và trung vị:
- Mức đồng thuận lớp:
- Số hộp phía bạn không ghép được:
- Số hộp phía đối chiếu không ghép được:
- Một điểm khác biệt cụ thể:
- Quy tắc hoặc hành động sửa phát sinh:
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?

- Số hộp ghép được:
48

- IoU trung bình và trung vị:
IoU trung bình = 0.768085; IoU trung vị = 0.80856.

- Mức đồng thuận lớp:
0.729167 (khoảng 72.92%).

- Số hộp phía bạn không ghép được:
78

- Số hộp phía đối chiếu không ghép được:
2

- Một điểm khác biệt cụ thể:
Phía tôi có 78 bounding box không ghép được với bộ đối chiếu, trong khi phía đối chiếu chỉ có 2 box không ghép được. Điều này cho thấy cần kiểm tra lại các box dư hoặc cách xác định phạm vi vật thể trong bài gán nhãn của tôi.

- Quy tắc hoặc hành động sửa phát sinh:
Đối chiếu lại từng box không ghép được với guideline và bộ tham chiếu; kiểm tra xem đó có phải vật thể thuộc phạm vi cần gán nhãn hay không, đồng thời sửa class và bounding box nếu phát hiện sai.

- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?
Vì mức đồng thuận chỉ cho biết hai bộ nhãn giống nhau ở mức nào, không chứng minh bộ nhãn đối chiếu hoặc nhãn của tôi là ground truth hoàn toàn chính xác. Hai bên vẫn có thể cùng mắc một lỗi, và vẫn còn các box không ghép được hoặc sai lớp cần được kiểm tra.

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

