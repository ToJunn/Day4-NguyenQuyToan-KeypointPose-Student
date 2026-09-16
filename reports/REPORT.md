# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Quý Toàn, Ngày lập báo cáo: 16/09/2026

> Báo cáo tổng hợp từ các tệp có trong dự án. Các mục thiếu nhật ký hoặc kết quả đối chiếu được ghi rõ, không suy diễn thành hoạt động đã thực hiện.

## 1. Nhãn của tôi

Nguồn theo template: [visibility_report.md](visibility_report.md) và [visibility_report.json](../outputs/visibility_report.json).

| Chỉ số | Giá trị trong báo cáo visibility đã lưu |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 335 / 118 / 23 |
| Thời gian trung bình mỗi ảnh | Chưa có nhật ký thời gian để tính |

**Đối chiếu với nhãn hiện tại:** đếm trực tiếp trong `dataset/labels/train` cho kết quả 20 ảnh, **29 skeleton**, với **v=2: 345, v=1: 125, v=0: 23**. Tổng 493 điểm bằng 29 × 17; 470 điểm có v > 0. File COCO `annotations/coco_keypoints/person_keypoints_default.json` cũng có 20 ảnh và 29 annotation. Vì vậy, bảng visibility đã lưu chưa đồng bộ với nhãn hiện tại; cần chạy lại công cụ visibility trước khi nộp chính thức. Các JSON nguồn được giữ nguyên.

Ba khớp có `%v=1` cao nhất trong báo cáo visibility đã lưu:

1. `left_ear`: 18/28, hiển thị **64%**.
2. `right_ear`: 14/28, hiển thị **50%**.
3. `left_eye`: 8/28, hiển thị **29%**; đồng hạng với `left_wrist` (8/28).

Trong nhãn hiện tại, thứ tự này vẫn giữ: `left_ear` 19/29, `right_ear` 14/29, `left_eye` và `left_wrist` cùng 9/29. Tỷ lệ bị che cao chưa đủ để khẳng định đây là những khớp khó gán nhất về mặt giải phẫu. Ví dụ, ở [train_01.jpg](../dataset/images/train/train_01.jpg), tóc của người phụ nữ che vùng tai và tay nằm sát khay pizza; đây là căn cứ trực quan cho khó khăn do che khuất. Ngược lại, hông có thể khó xác định vị trí dưới quần áo dù tỷ lệ v=1 không đứng đầu; chưa có nhật ký cá nhân để xếp hạng mức độ khó khi thao tác.

## 2. Chấm với gold

Nguồn: [eval_vs_gold.json](../outputs/eval_vs_gold.json). File `eval_vs_gold (2).json` có nội dung JSON tương đương; tên có hậu tố `(2)` không chứng minh đó là lần chấm sau rework. Vì chưa xác định được mốc trước/sau, bảng giữ hai cột của template và bổ sung kết quả hiện có.

| Chỉ số | Trước rework | Sau rework | Kết quả đã lưu |
| --- | ---: | ---: | ---: |
| OKS trung bình | Chưa xác định | Chưa xác định | 0.9382 |
| OKS@0.50 | Chưa xác định | Chưa xác định | 1.0 |
| OKS@0.75 | Chưa xác định | Chưa xác định | 1.0 |
| Lỗi `dao_trai_phai` | Chưa xác định | Chưa xác định | 0 |
| Lỗi `nham_nguoi` | Chưa xác định | Chưa xác định | 0 |
| Lỗi `xoa_khop_bi_che` | Chưa xác định | Chưa xác định | 0 |

Kết quả ghép đủ **29/29 người**, không thiếu và không thừa người. Có **6** phát hiện `lech_nhe`, **54** phát hiện `co_khac_gold` và **78** phát hiện `gold_khong_gan_nhan`. Theo chi tiết trong JSON, khác cờ v=1/v=2 ở vị trí đúng không bị trừ OKS; khớp gold có v=0 bị loại khỏi phép tính. Vì vậy, OKS cao không đồng nghĩa toàn bộ cờ visibility đã thống nhất với gold.

**Tôi đã sửa gì giữa hai lần chạy:** chưa có lịch sử chỉnh nhãn và hai kết quả độc lập để xác nhận. Những mục dưới đây là danh sách cần kiểm tra từ kết quả chấm, không phải các thao tác đã hoàn thành. Số người theo trường `your_person` của công cụ:

| Ảnh | Người thứ | Keypoint | Phát hiện và thao tác cần kiểm tra |
| --- | ---: | --- | --- |
| train_01.jpg | 1 | right_wrist | Lệch 26 px; kiểm tra lại tâm cổ tay theo ảnh |
| train_03.jpg | 2 | right_hip | Lệch 39 px; kiểm tra vị trí hông |
| train_13.jpg | 2 | left_ankle, right_ankle | Lệch lần lượt 20 px, 19 px; kiểm tra hai cổ chân |
| train_16.jpg | 1 | nose, right_eye | Lệch lần lượt 16 px, 14 px; kiểm tra mốc mặt |

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Kết quả đánh giá đã lưu không phát hiện lỗi đảo trái/phải trong toàn bộ 20 ảnh. Đây là kết luận của công cụ trên lần đánh giá đó; không có ảnh bị gắn loại lỗi này để phân tích dễ/khó. Việc đổi vị trí hiển thị trong SVG skeleton không được tính là bằng chứng đã sửa nhãn của một ảnh cụ thể.

## 3. Kiểm chéo

Bạn cùng nhóm: **chưa cung cấp**.

Trường `comparison` trong cả hai file visibility JSON đều là `null`, chưa có bảng đếm của người thứ hai. Hai bản visibility có cùng dữ liệu không đủ để xem là bằng chứng kiểm chéo.

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| Chưa xác định | Chưa có bảng so sánh | Chưa có dữ liệu | Chưa tính được | Chưa đủ bằng chứng |

**Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:** chưa có nội dung được điền trong phần luật nhóm và kiểm chéo của [GUIDELINE_MINI.md](../GUIDELINE_MINI.md), nên chưa thể ghi nhận một quy tắc đã được thống nhất.

Đề xuất để nhóm xem xét: khi khớp bị vật thể hoặc cơ thể khác che nhưng vị trí suy ra từ đoạn chi liền kề vẫn nằm trong ảnh, giữ điểm và chọn v=1; khi khớp nằm ngoài biên ảnh, chọn v=0. Cần kèm ảnh minh họa và xác nhận của người kiểm chéo trước khi ghi thành luật nhóm.

## 4. Model

Nguồn: [eval_model.json](../outputs/eval_model.json). Các giá trị dưới đây giữ nguyên độ chính xác trong JSON; chênh lệch bằng sau fine-tune trừ baseline.

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.0 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

Ngoài bảng của template, `box_mAP50` giảm từ 0.9785 xuống 0.96, chênh -0.0185. Dự án có 20 ảnh train và 10 ảnh test; `data.yaml` dùng thư mục test làm tập `val`. File kết quả chỉ chứa chỉ số tổng hợp, chưa đủ thông tin xác minh checkpoint, cấu hình chạy và chất lượng từng ảnh.

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu?** Tăng từ 0.6853 lên 0.6908, tức **0.0055**, tương đương **0.55 điểm phần trăm**. Precision tăng 0.0058, còn recall và pose_mAP50 giữ nguyên. Đây là cải thiện nhỏ trên tập đánh giá đã ghi nhận; chưa đủ bằng chứng kết luận 20 ảnh đã dạy model kiến thức cụ thể nào mà COCO chưa có. Đồng thời, box_mAP50-95 giảm 0.0078 cho thấy chỉ số định vị hộp giảm trong lần đánh giá này, nhưng chưa chứng minh nguyên nhân là nhãn sai.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu?** So cùng ngưỡng 50–95: baseline chênh **0.1266** (0.8119 − 0.6853), sau fine-tune chênh **0.1133** (0.8041 − 0.6908). Chỉ số hộp cao hơn chỉ số pose, phù hợp với việc xác định vùng chứa người thuận lợi hơn định vị chính xác nhiều khớp bị che hoặc nhỏ. Tuy nhiên, box AP và pose AP dùng tiêu chí ghép khác nhau nên khoảng chênh không phải phép đo trực tiếp, tuyệt đối về độ khó.

3. **Một ảnh test model đoán sai và loại lỗi:** chưa xác định được từ các tệp hiện có. `eval_model.json` không lưu dự đoán theo ảnh, còn notebook không có output đã lưu để kiểm tra hình dự đoán. Cần ảnh overlay hoặc tọa độ model để chọn một ảnh test và phân loại lệch nhẹ / đảo trái-phải / nhầm người / trượt hẳn; không dùng lỗi nhãn-vs-gold để thay thế lỗi của model.

4. **Ảnh có OKS thấp nhất giữa nhãn của tôi và model; ai đúng?** Chưa có kết quả OKS nhãn-vs-model nên chưa thể xác định. Kết quả hiện có là **nhãn-vs-gold**: `train_13.jpg`, người thứ 2, thấp nhất theo từng người với **OKS 0.8198**. Ảnh này cũng có trung bình OKS của các người thấp nhất, khoảng **0.88123**. Các số này không cho phép kết luận model đúng hay người gán đúng.

5. **Ảnh gán tệ nhất có cùng ảnh model đoán tệ nhất không?** Chưa thể đối chiếu. `train_13.jpg` thấp nhất trong kết quả nhãn-vs-gold; chưa có xếp hạng lỗi model theo ảnh. Hơn nữa, tập nhãn đang chấm gold là train, còn kết quả model tổng hợp gắn với quy trình đánh giá test; cần dự đoán trên cùng tập ảnh và cùng tiêu chí trước khi so sánh.

## 5. Một rule evidence bạn đã dùng

Ví dụ kiểm tra được từ ảnh và nhãn hiện có: **`train_01.jpg`, người thứ 1 trong file YOLO (người đàn ông bên phải), `left_ankle`**. Ảnh chỉ cho thấy phần thân và chân đến mép dưới, không chứa vùng cổ chân của người này. Vì vị trí giải phẫu của cổ chân nằm dưới biên ảnh, nhãn hiện tại ghi `(x, y, v) = (0, 0, 0)`, phù hợp với lựa chọn **v=0**. Nếu cổ chân còn trong ảnh nhưng bị vật thể che thì cần giữ điểm ước lượng với v=1; trường hợp này không có căn cứ để đặt một điểm cổ chân bên trong khung.

Ảnh đối chiếu: [train_01.jpg](../dataset/images/train/train_01.jpg). Nhãn: [train_01.txt](../dataset/labels/train/train_01.txt), dòng người thứ 1. Đây là phân tích lại từ dữ liệu đã lưu, không phải nhật ký quyết định tại thời điểm gán nhãn.
