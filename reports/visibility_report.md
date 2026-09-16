# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 29 skeleton, trung bình 15.86 khớp có v > 0 mỗi người
- Tổng: v=2 312 | v=1 148 | v=0 33

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 20 | 9 | 0 | 31% |
| 1 | left_eye | 19 | 10 | 0 | 34% |
| 2 | right_eye | 19 | 10 | 0 | 34% |
| 3 | left_ear | 7 | 22 | 0 | 76% |
| 4 | right_ear | 5 | 24 | 0 | 83% |
| 5 | left_shoulder | 28 | 1 | 0 | 3% |
| 6 | right_shoulder | 26 | 3 | 0 | 10% |
| 7 | left_elbow | 25 | 4 | 0 | 14% |
| 8 | right_elbow | 23 | 6 | 0 | 21% |
| 9 | left_wrist | 20 | 9 | 0 | 31% |
| 10 | right_wrist | 19 | 9 | 1 | 31% |
| 11 | left_hip | 20 | 7 | 2 | 24% |
| 12 | right_hip | 18 | 9 | 2 | 31% |
| 13 | left_knee | 18 | 5 | 6 | 17% |
| 14 | right_knee | 15 | 8 | 6 | 28% |
| 15 | left_ankle | 17 | 4 | 8 | 14% |
| 16 | right_ankle | 13 | 8 | 8 | 28% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
