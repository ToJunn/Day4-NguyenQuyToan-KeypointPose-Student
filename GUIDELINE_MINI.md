# Mini guideline - Keypoint & Pose

Nhóm: chưa cung cấp | Người gán: Nguyễn Quý Toàn (theo tên thư mục bài nộp) | Ngày cập nhật: 16/09/2026

> Bản hướng dẫn được tổng hợp lại từ ảnh, nhãn và kết quả đánh giá hiện có; không phải nhật ký được ghi ngay trong lúc gán. Quy tắc ở mục 2 là đề xuất áp dụng cá nhân, chưa có xác nhận thống nhất nhóm. Ảnh minh họa dưới đây là ảnh gốc của dataset; cần bổ sung screenshot CVAT có điểm và trạng thái visibility để hoàn thiện bằng chứng thao tác.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (dự thảo chờ kiểm chéo)

| Tình huống | Quy tắc đề xuất | Vì sao và ảnh đối chiếu |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Đặt tại vị trí khớp hông giải phẫu dựa trên thân và đùi, không mặc định lấy cạp quần hoặc mép áo. Khi mốc hông bị che và phải suy ra vị trí, dùng v=1; không dùng v=0 chỉ vì không thấy da. | Quần áo không xác định chính xác tâm khớp. Xem người đàn ông bên phải trong ảnh A. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu còn xác định trực tiếp được mốc tai thì v=2; nếu mốc bị che nhưng suy ra được từ đầu và tai đối diện thì v=1. Đánh giá từng tai riêng, không gán cùng cờ cho cả hai theo thói quen. | Tóc có thể che một bên nhiều hơn bên kia. Xem người phụ nữ trong ảnh B. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Xét từng khớp: vị trí còn trong khung nhưng bị che là v=1; vị trí ngoài khung là v=0. Giữ đủ 17 phần tử, không kéo điểm ngoài khung vào sát mép ảnh. | Khớp ngoài khung khác khớp bị che. Hai cổ chân của người đàn ông trong ảnh C nằm ngoài ảnh. |
| Cổ tay nằm sau tay lái / sau thân mình | Theo dõi cẳng tay tới cổ tay của đúng người. Nếu mốc cổ tay bị vật thể hoặc cơ thể che nhưng vẫn ở trong ảnh thì đặt điểm ước lượng và dùng v=1; không đặt điểm lên mép vật che. | Ảnh D minh họa vật che là khay pizza; cùng nguyên tắc khi vật che là tay lái hoặc thân người. |
| Hai người chồng lên nhau | Theo từng chuỗi vai–khuỷu–cổ tay và hông–gối–cổ chân của một người. Khớp người phía sau bị người phía trước che vẫn là v=1 nếu nằm trong ảnh; không lấy khớp người phía trước để nối vào skeleton phía sau. | Ảnh E có hai người chồng lấp, một số đoạn tay và chân khó theo dõi. |
| Người nhỏ đến mức nào thì không gán nữa | Chưa có ngưỡng kích thước được phê duyệt; không tự bỏ một người chỉ vì nhỏ hoặc mờ. Theo yêu cầu bài, mọi người đều có skeleton; trường hợp không xác định được mốc cần đánh dấu để hỏi người hướng dẫn, không dùng v=0 chỉ vì ảnh mờ. | Người ở nền ảnh F nhỏ và mờ hơn người phía trước; cần kiểm tra đủ người trước khi chỉnh từng khớp. |

**Ảnh A — Hông dưới quần áo, `train_01.jpg`:** người đàn ông ở bên phải; nhãn hiện có đặt hai hông v=1. Cần kiểm chéo cách nhận diện mốc hông vì kết quả gold ghi cờ khác ở hai điểm này.

![Ảnh A: Hông dưới quần áo](dataset/images/train/train_01.jpg)

**Ảnh B — Tai bị tóc che, `train_01.jpg`:** người phụ nữ ở bên trái; hai tai trong nhãn hiện có đều v=1. Ảnh gốc giúp kiểm tra mức che khuất, nhưng không thay thế ảnh chụp điểm gán trong CVAT.

![Ảnh B: Tai bị tóc che](dataset/images/train/train_01.jpg)

**Ảnh C — Người bị cắt bởi mép dưới, `train_01.jpg`:** vùng cổ chân của hai người không nằm trong ảnh. Không suy từ trạng thái cổ chân để tự động đặt các khớp phía trên thành v=0.

![Ảnh C: Cổ chân ngoài khung](dataset/images/train/train_01.jpg)

**Ảnh D — Cổ tay sát vật che, `train_01.jpg`:** tay người đàn ông đỡ khay pizza. Nhãn `right_wrist` hiện có v=1 và công cụ báo lệch vị trí 26 px, nên cần kiểm tra cả cờ lẫn tọa độ.

![Ảnh D: Cổ tay và khay pizza](dataset/images/train/train_01.jpg)

**Ảnh E — Hai người chồng lấp, `train_03.jpg`:** người phía trước đội mũ, mặc áo khoác; người phía sau bị che một phần. Xe đạp cũng che một số vùng chân, nên phải lần theo từng người trước khi đặt điểm.

![Ảnh E: Hai người chồng lấp](dataset/images/train/train_03.jpg)

**Ảnh F — Người nhỏ ở nền, `train_13.jpg`:** có người đứng phía sau và một người nhỏ hơn ở phía trái ảnh. Đây là ca cần kiểm tra phạm vi gán, không phải căn cứ đặt ngưỡng loại bỏ tùy ý.

![Ảnh F: Người nhỏ ở nền](dataset/images/train/train_13.jpg)

## 3. Ba ca mơ hồ đã gặp (đối chiếu lại từ nhãn hiện có)

Số thứ tự người dưới đây là thứ tự dòng trong file YOLO tương ứng, không phải ID track của CVAT. Các quyết định được mô tả từ nhãn đã lưu; chưa có nhật ký xác nhận suy nghĩ tại thời điểm gán.

### Ca 1 - ảnh `train_01.jpg`, người thứ `2`, khớp `left_ear`

- **Mơ hồ ở chỗ nào:** tóc của người phụ nữ che vùng tai; không thể chỉ dựa vào việc thấy phần đầu để coi mốc tai là nhìn rõ.
- **Quyết định trong nhãn hiện có:** giữ điểm `left_ear`, chọn v=1.
- **Vì sao:** tai vẫn thuộc vùng đầu nằm trong khung; có thể suy vị trí từ mặt và đường viền đầu, nên không chọn v=0 do tóc che.
- **Nếu quyết ngược lại:** chọn v=0 sẽ loại một khớp còn trong khung khỏi phần giám sát vị trí ở quy trình dùng mask v>0; chọn v=2 làm cờ visibility không phản ánh mức che khuất.

### Ca 2 - ảnh `train_01.jpg`, người thứ `1`, khớp `left_ankle`

- **Mơ hồ ở chỗ nào:** người đàn ông bị cắt ở mép dưới; cần phân biệt phần chân không xuất hiện vì ngoài ảnh với phần chân bị đồ vật che.
- **Quyết định trong nhãn hiện có:** chọn v=0, tọa độ xuất YOLO là `(0, 0)`; giữ phần tử khớp trong bộ 17 điểm.
- **Vì sao:** ảnh không chứa vùng cổ chân, trong khi đùi kéo tới biên dưới; không có căn cứ đặt cổ chân tại một vị trí trong khung.
- **Nếu quyết ngược lại:** đặt v=1 kèm một điểm ở mép ảnh sẽ tạo mục tiêu tọa độ không có căn cứ và làm sai hình học của chân.

### Ca 3 - ảnh `train_03.jpg`, người thứ `2`, khớp `left_elbow`

- **Mơ hồ ở chỗ nào:** người phía sau bị người mặc áo khoác phía trước che; các đoạn tay gần nhau dễ dẫn tới nối nhầm người.
- **Quyết định trong nhãn hiện có:** giữ `left_elbow`, chọn v=1.
- **Vì sao:** khuỷu tay thuộc người phía sau vẫn nằm trong khung, dù không nhìn rõ trực tiếp; cần suy từ vai và hướng cánh tay của chính người đó.
- **Nếu quyết ngược lại:** xóa hoặc đặt v=0 làm mất giám sát khớp bị che; lấy khuỷu của người phía trước sẽ tạo skeleton trộn hai người. Tọa độ suy ra cần được kiểm tra lại bằng overlay, không coi cờ v=1 là bằng chứng tọa độ đã đúng.

## 4. Sau khi so visibility report với bạn cùng nhóm

- **Bạn cùng nhóm:** chưa cung cấp; chưa có dữ liệu kiểm chéo độc lập.
- **Khớp lệch `%v=1` nhiều nhất:** chưa xác định. Cả hai file visibility JSON hiện có đều có `comparison: null`, không thể tính cột của người đối chiếu và độ lệch.
- **Nguyên nhân là guideline chưa rõ hay một trong hai bên gán sai:** chưa kết luận được. Kết quả `eval_vs_gold.json` có 54 phát hiện `co_khac_gold`; đây là khác cờ với gold, không phải kết quả kiểm chéo giữa hai học viên.
- **Luật mới sau khi thống nhất:** chưa có xác nhận nhóm. Đề xuất cần thảo luận: mốc khớp bị che nhưng vẫn trong ảnh thì giữ tọa độ ước lượng và v=1; chỉ chọn v=0 khi vị trí khớp ở ngoài khung theo luật của bài. Kèm ảnh chụp CVAT cho từng trường hợp trước khi ghi là đã thống nhất.
- **Dữ liệu cần đồng bộ trước khi kiểm chéo:** visibility đã lưu ghi 28 skeleton, v=2/v=1/v=0 là 335/118/23; đếm nhãn hiện tại có 29 skeleton, tương ứng 345/125/23. Cần tạo lại visibility report từ nhãn hiện tại rồi mới so với bạn cùng nhóm.

Nguồn đối chiếu: [báo cáo tổng hợp](reports/REPORT.md), [kết quả gold](outputs/eval_vs_gold.json), [nhãn train_01](dataset/labels/train/train_01.txt), [nhãn train_03](dataset/labels/train/train_03.txt).