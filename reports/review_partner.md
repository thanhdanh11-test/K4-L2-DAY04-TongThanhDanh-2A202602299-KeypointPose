# Kiểm chéo - lỗi tìm được trong bài của người khác

Người gán: ______   Người kiểm: Tống Thành Danh (2A202602299)   Ngày: ______

> **Trạng thái: chưa thực hiện được.** Chưa có bài của bạn cùng nhóm để kiểm.
> Khung dưới đây đã dựng sẵn theo `reports/REVIEWER_CHECKLIST.md`; chỉ cần điền khi có đường dẫn.

## Lệnh chạy trước khi soi bằng mắt

```bash
BAI=<đường dẫn dataset/labels/train của họ>
python3 tools/check_pose_labels.py --images dataset/images/train --labels "$BAI"
python3 tools/visualize_pose.py --images dataset/images/train --labels "$BAI" --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare "$BAI" \
    --markdown reports/visibility_compare.md
```

## Reviewer checklist

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☐ | |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☐ | |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☐ | |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☐ | |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☐ | |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☐ | |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☐ | |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☐ | |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☐ | |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☐ | |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☐ | |

## Lỗi tìm được

Mỗi dòng một lỗi, đủ năm cột - người sửa phải mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| | | | | |
| | | | | |
| | | | | |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này:
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**?

---

## Phụ lục — ba cảnh báo của chính bài tôi, để người kiểm đối chiếu

Người kiểm bài tôi sẽ gặp 7 cảnh báo từ `check_pose_labels.py`. Cả 7 đều thuộc 2 nhóm dưới đây
và tôi đã kiểm từng cái:

| Cảnh báo | Ảnh | Kết luận của tôi |
| --- | --- | --- |
| "có N khớp v=0 trong khi cả người nằm gọn giữa ảnh" | `train_01` ×2, `train_04` ×2, `train_10`, `train_11` | **Dương tính giả.** Các khớp đó (gối, cổ chân, cổ tay) thật sự nằm ngoài mép ảnh. Script cảnh báo vì bbox do CVAT tính chỉ từ các điểm **có chấm**, nên hộp dừng ở hông và không chạm mép ảnh. |
| "left_shoulder/right_shoulder nằm ngược chiều so với hai mắt" | `train_02` | **Dương tính giả.** Ảnh chụp **sau lưng** người đạp xe: nhìn từ gáy thì hai mắt đảo chiều so với hai vai. Heuristic giả định mặt quay về phía camera. Bằng chứng xác định chiều: moay-ơ sau có cần gạt QR và không thấy líp → camera ở mặt trái xe → chi trái ở phía x lớn. |

Nếu người kiểm không đồng ý với hai kết luận này, đó là **bất đồng guideline**, không phải lỗi
thao tác — ghi vào mục "Hai câu kết luận" ở trên.
