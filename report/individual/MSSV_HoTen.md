# Báo cáo thực hành Day 3

- Mã học viên theo lớp: `2A202603062`
- Ngày / CVAT local: `12/11/2026` / CVAT local
- Công cụ đã dùng: CVAT local; `auto_task.py` với SegFormer B1 (Cityscapes) và YOLO26n-seg để tạo nhãn gợi ý. Nhãn gợi ý cần được kiểm tra và sửa trong CVAT.

## 1. Bài đã nộp

Các ZIP hiện có trong `submissions/` là output/pre-label đã tạo trong quá trình làm việc. Chỉ ghi nhận là file nộp sau khi đã Save và export bản cuối từ CVAT. Theo thông tin hiện có, CP4 và CP5 đã hoàn thành 1/1 ảnh; chưa xác nhận trạng thái export cuối.

| Task | File ZIP đúng tên trong `submissions/` | Hoàn thành mấy ảnh đã vẽ, Save | Điểm tối đa (coach chấm sau) |
|---|---|---:|---:|
| easy_semantic | chưa có bản export cuối từ CVAT (preview: `easy_semantic.zip`) | chưa xác nhận / 3 | 20 |
| medium_instance | chưa có bản export cuối từ CVAT (preview: `medium_instance.zip`) | chưa xác nhận / 3 | 32 |
| hard_panoptic | chưa có bản export cuối từ CVAT (preview: `hard_panoptic.zip`) | chưa xác nhận / 2 | 30 |
| cp1_holes | chưa có bản export cuối từ CVAT (preview: `cp1_holes.zip`) | chưa xác nhận / 1 | 3 |
| cp2_slice | chưa có bản export cuối từ CVAT (preview: `cp2_slice.zip`) | chưa xác nhận / 1 | 3 |
| cp5_occlusion | chưa xác nhận bản export cuối (preview: `cp5_occlusion.zip`) | 1 / 1 theo thông tin đã ghi | 3 |
| cp3_thin | chưa có bản export cuối từ CVAT (preview: `cp3_thin.zip`) | chưa xác nhận / 1 | 3 |
| cp4_curb | chưa xác nhận bản export cuối (preview: `cp4_curb.zip`) | 1 / 1 theo thông tin đã ghi | 3 |
| cp6_coverage | chưa có bản export cuối từ CVAT (preview: `cp6_coverage.zip`) | chưa xác nhận / 1 | 3 |
| **Tổng tối đa** |  |  | **100** |

Ghi chú lỗi import đã gặp: CP3 và CP4 báo `Label 'background' is not registered for this task` khi import Segmentation Mask 1.1. Chưa xác nhận đã xử lý xong và export bản cuối; cần hỏi coach cách xử lý label nền kỹ thuật mà không đưa nó thành class chấm bài.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: **chưa ghi lại; cần bổ sung theo object thực tế đã tự vẽ trong CVAT**.
- Class và quy tắc tôi dùng để chọn biên: dùng đúng tên class trong `classes.json`; vẽ sát phần nhìn thấy, không suy đoán phần bị che.
- Nếu dùng gợi ý sau đó: có dùng nhãn tự động làm gợi ý. Với object Medium đầu tiên, vùng gợi ý sai/đúng và hành động sửa cụ thể **chưa được ghi lại; cần bổ sung trước khi nộp**.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `cp5_occlusion`, ảnh `000000336232.jpg`, góc dưới bên phải — hai taxi vàng sát nhau.
- Lỗi thuộc loại: gộp-tách instance.
- Bằng chứng tôi nhìn thấy: đường viền xanh bao cả hai taxi như một mask, trong khi ảnh cho thấy hai xe riêng biệt: một xe phía trước và một xe phía sau.
- Quy tắc và hành động sửa: phải tách thành hai object `car` riêng; mỗi mask chỉ theo phần xe nhìn thấy, không đoán phần bị che. **Cần xác nhận đã thực hiện thao tác tách trong CVAT.**
- Sau sửa đã Save và export lại chưa? **Chưa xác nhận; không ghi là đã sửa xong cho đến khi Save và export bản cuối.**
- Kết quả liên quan lỗi này: **chưa có điểm**; hiện không có ground truth CP5 để tính metric trước/sau.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
|---|---|---|---|
| `cp4_curb`, dải cỏ có túi rác giữa bó vỉa và vỉa hè bê tông | Gộp dải cỏ vào `sidewalk`, hoặc để trống vì task chỉ có `road` và `sidewalk` | Ranh `road`–`sidewalk` theo chức năng và bó vỉa; dải cỏ không phải bề mặt đi bộ, túi rác cũng không thuộc hai class này | Để trống dải cỏ và túi rác; chỉ tô phần mặt đường và vỉa hè thuộc class được phép |
| `cp4_curb`, phần bê tông dưới xe trắng đỗ bên phải | `sidewalk`, hoặc lối xe ra vào không thuộc `sidewalk` | Cần xem vùng bê tông có liên tục với lối đi bộ/bó vỉa hay phục vụ xe đi qua; vị trí dưới xe làm ranh khó nhìn | Chưa đủ chứng cứ từ mô tả để chốt; hỏi coach nếu xem ảnh phóng to vẫn không xác định được chức năng |
| `cp5_occlusion`, xe máy bị người ngồi phía trước che một phần | Gộp người và xe máy thành một object, hoặc tách thành hai object | Người và xe máy là hai vật khác class; vật bị che vẫn là một instance nhưng chỉ vẽ phần nhìn thấy | Tách thành hai object: một `motorcycle`, một `person`; không vẽ phần bị che |
