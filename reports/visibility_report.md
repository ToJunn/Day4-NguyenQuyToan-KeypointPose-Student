# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 16.18 khớp có v > 0 mỗi người
- Tổng: v=2 335 | v=1 118 | v=0 23

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 5 | 0 | 18% |
| 1 | left_eye | 20 | 8 | 0 | 29% |
| 2 | right_eye | 21 | 7 | 0 | 25% |
| 3 | left_ear | 10 | 18 | 0 | 64% |
| 4 | right_ear | 14 | 14 | 0 | 50% |
| 5 | left_shoulder | 25 | 3 | 0 | 11% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 22 | 6 | 0 | 21% |
| 8 | right_elbow | 25 | 3 | 0 | 11% |
| 9 | left_wrist | 20 | 8 | 0 | 29% |
| 10 | right_wrist | 20 | 7 | 1 | 25% |
| 11 | left_hip | 21 | 7 | 0 | 25% |
| 12 | right_hip | 23 | 5 | 0 | 18% |
| 13 | left_knee | 18 | 7 | 3 | 25% |
| 14 | right_knee | 20 | 5 | 3 | 18% |
| 15 | left_ankle | 13 | 7 | 8 | 25% |
| 16 | right_ankle | 13 | 7 | 8 | 25% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
