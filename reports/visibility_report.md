# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 29 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 314 | v=1 148 | v=0 31

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 8 | 0 | 28% |
| 1 | left_eye | 19 | 10 | 0 | 34% |
| 2 | right_eye | 19 | 10 | 0 | 34% |
| 3 | left_ear | 7 | 22 | 0 | 76% |
| 4 | right_ear | 5 | 24 | 0 | 83% |
| 5 | left_shoulder | 28 | 1 | 0 | 3% |
| 6 | right_shoulder | 26 | 3 | 0 | 10% |
| 7 | left_elbow | 25 | 4 | 0 | 14% |
| 8 | right_elbow | 23 | 6 | 0 | 21% |
| 9 | left_wrist | 22 | 7 | 0 | 24% |
| 10 | right_wrist | 18 | 10 | 1 | 34% |
| 11 | left_hip | 20 | 8 | 1 | 28% |
| 12 | right_hip | 18 | 10 | 1 | 34% |
| 13 | left_knee | 18 | 5 | 6 | 17% |
| 14 | right_knee | 15 | 8 | 6 | 28% |
| 15 | left_ankle | 16 | 5 | 8 | 17% |
| 16 | right_ankle | 14 | 7 | 8 | 24% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
