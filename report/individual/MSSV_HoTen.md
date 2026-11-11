# Báo cáo thực hành Day 3

- Mã học viên theo lớp: `2A202603062`
- Ngày / CVAT local: `12/11/2026` / CVAT local
- Công cụ đã dùng: CVAT local; nhãn gợi ý từ `auto_task.py` (SegFormer B1 và YOLO26n-seg), sau đó kiểm tra và chỉnh trong CVAT.

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
|---|---|---:|---:|
| `easy_semantic` | `easy_semantic.zip` | 3/3 | 20 |
| `medium_instance` | `medium_instance.zip` | 3/3 | 32 |
| `hard_panoptic` | `hard_panoptic.zip` | 2/2 | 30 |
| `cp1_holes` | `cp1_holes.zip` | 1/1 | 3 |
| `cp2_slice` | `cp2_slice.zip` | 1/1 | 3 |
| `cp5_occlusion` | `cp5_occlusion.zip` | 1/1 | 3 |
| `cp3_thin` | `cp3_thin.zip` | 1/1 | 3 |
| `cp4_curb` | `cp4_curb.zip` | 1/1 | 3 |
| `cp6_coverage` | `cp6_coverage.zip` | 1/1 | 3 |
| **Tổng tối đa** |  | **14/14 ảnh** | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium: `000000181542.jpg`, người ở gần giữa-phải ảnh (`person`).
- Class và quy tắc tôi dùng để chọn biên: Chọn `person`; bám theo đường bao phần cơ thể nhìn thấy và dừng ở ranh với xe/nền, không tự vẽ phần bị che.
- Nếu dùng gợi ý sau đó: Dùng gợi ý model làm điểm bắt đầu, rồi kiểm tra và chỉnh đường bao theo ảnh. Với object cụ thể này, tôi không lưu lại thứ tự thao tác trước/sau khi xem gợi ý nên không khẳng định đây là object đầu tiên đã tự vẽ trước gợi ý.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `cp5_occlusion`, ảnh đường phố, góc dưới bên phải — hai taxi vàng sát nhau.
- Lỗi thuộc loại: gộp-tách.
- Bằng chứng tôi nhìn thấy: Một đường viền mask bao cả hai taxi, dù ảnh thể hiện hai xe riêng biệt.
- Quy tắc và hành động sửa: Tách thành hai object `car`; mỗi mask chỉ theo phần nhìn thấy của xe tương ứng, không vẽ xuyên phần bị che.
- Sau sửa đã Save và export lại chưa? Có. Đã chỉnh trong CVAT, Save và export lại ZIP.
- Kết quả Summary/scorer cho lỗi này: Chưa có điểm đối chiếu ground truth.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
|---|---|---|---|
| `cp4_curb`, dải cỏ có túi rác giữa bó vỉa và vỉa hè bê tông | Gộp vào `sidewalk` hoặc để ngoài hai class | Task chỉ có `road` và `sidewalk`; dải cỏ và túi rác không phải mặt đường hay phần vỉa hè đi bộ | Để ngoài mask `road`/`sidewalk`; chỉ gán vùng phù hợp rõ ràng với class. |
| `cp4_curb`, phần bê tông dưới xe trắng đỗ bên phải | `sidewalk` hoặc lối xe ra vào | Xét chức năng, tính liên tục của vỉa hè và bó vỉa; màu bê tông riêng lẻ chưa đủ kết luận | Nếu ảnh không thể hiện rõ chức năng, hỏi coach thay vì ép gán lớp. |
| `cp5_occlusion`, xe máy bị người ngồi che một phần | Gộp người với xe máy hoặc tách riêng | Người và xe máy là hai vật; vật bị che vẫn là một instance nhưng chỉ vẽ phần nhìn thấy | Tách người và xe máy thành hai object riêng; không đoán phần xe bị che. |
