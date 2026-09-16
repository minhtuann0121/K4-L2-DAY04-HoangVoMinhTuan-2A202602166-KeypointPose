# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.79 khớp có v > 0 mỗi người
- Tổng: v=2 356 | v=1 102 | v=0 35

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 7 | 0 | 24% |
| 1 | left_eye | 22 | 7 | 0 | 24% |
| 2 | right_eye | 19 | 9 | 1 | 31% |
| 3 | left_ear | 16 | 12 | 1 | 41% |
| 4 | right_ear | 17 | 11 | 1 | 38% |
| 5 | left_shoulder | 27 | 2 | 0 | 7% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 23 | 5 | 1 | 17% |
| 8 | right_elbow | 26 | 3 | 0 | 10% |
| 9 | left_wrist | 20 | 8 | 1 | 28% |
| 10 | right_wrist | 21 | 7 | 1 | 24% |
| 11 | left_hip | 23 | 6 | 0 | 21% |
| 12 | right_hip | 22 | 6 | 1 | 21% |
| 13 | left_knee | 18 | 5 | 6 | 17% |
| 14 | right_knee | 18 | 5 | 6 | 17% |
| 15 | left_ankle | 17 | 4 | 8 | 14% |
| 16 | right_ankle | 17 | 4 | 8 | 14% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
