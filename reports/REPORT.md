# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Tống Thành Danh (2A202602299)   Nhóm: SOLO   Ngày: 2026-09-16

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 312 / 148 / 33 |
| Thời gian trung bình mỗi ảnh | _(điền sau: tổng thời gian gán / 20)_ |

Tổng khớp: 493 = 17 × 29 — không skeleton nào bị xoá bớt điểm.
Trung bình 15.86 khớp có `v > 0` mỗi người (`outputs/visibility_report.json`).

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `right_ear` — 83% (5 khớp `v=2` / 24 khớp `v=1`)
2. `left_ear` — 76% (7 / 22)
3. `left_eye` và `right_eye` — 34% (19 / 10) *(đồng hạng)*

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

**Không hoàn toàn.** Hai cái tai đứng đầu bảng vì **hay bị che**, không vì khó xác định vị trí:
tai nằm ở một vị trí giải phẫu rất ổn định (ngang đuôi mắt, sát mép đầu), nên khi bị tóc hoặc
mũ bảo hiểm phủ tôi vẫn đặt chấm nhanh và chắc tay. Bộ 20 ảnh này có tới 7 người đội mũ
(`train_04` ×2, `train_06`, `train_09`, `train_12`, `train_15` ×2, `train_18`, `train_20`) và
4 người quay lưng hẳn (`train_02`, `train_09`, `train_14` ng1, `train_16` ng2, `train_19` ng2) —
đó là toàn bộ nguyên nhân của con số 83%.

Khớp **thật sự khó** với tôi lại có `%v=1` thấp: **`left_shoulder` chỉ 3%** nhưng là khớp tôi
phải sửa nhiều nhất, vì nó quyết định trái/phải cho cả nửa thân trên. Ở `train_02` tôi phải đi
tìm bằng chứng cơ khí (cần gạt QR ở moay-ơ, không thấy líp) mới dám kết luận camera đứng bên
trái người. Tương tự **hông** (`%v=1` 24-31%) khó vì không có bề mặt nhìn thấy được trên người
mặc quần áo — nhưng vì tôi chọn luật "quần áo không phải vật che" nên nó ra `v=2`, và độ khó đó
**không hiện lên trong bảng đếm**. Đây chính là giới hạn của visibility report: nó đo *bị che*,
không đo *khó*.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | | |
| OKS@0.50 | | |
| OKS@0.75 | | |
| Lỗi `dao_trai_phai` | | |
| Lỗi `nham_nguoi` | | |
| Lỗi `xoa_khop_bi_che` | | |

> **Chưa chạy được:** protected release chưa mở, `gold/` còn trống. Lệnh sẽ chạy:
> `python3 tools/evaluate_pose_annotations.py --pred dataset/labels/train --gold gold/labels/train --images dataset/images/train --out outputs/eval_vs_gold.json`

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

-
-
-

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

_(Điền sau khi có gold.)_

Ghi chú trước khi chấm: `tools/check_pose_labels.py` báo **1 cảnh báo nghi đảo trái/phải** ở
`train_02`. Tôi đã kiểm và giữ nguyên — xem Ca 1 trong `GUIDELINE_MINI.md`: đó là chữ ký của
ảnh chụp sau lưng, không phải lỗi. Nếu gold cũng không báo `dao_trai_phai` ở ảnh này thì kết
luận đó đúng.

## 3. Kiểm chéo

Bạn cùng nhóm: _(chưa có)_

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

> **Chưa chạy được:** chưa có bài của bạn cùng nhóm. Lệnh sẽ chạy:
> `python3 tools/visibility_report.py --labels dataset/labels/train --compare <đường dẫn bài của họ> --markdown reports/visibility_compare.md`

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

-

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | | | |
| pose_mAP50-95 | | | |
| pose_precision | | | |
| pose_recall | | | |
| box_mAP50-95 | | | |

> **Chưa chạy:** Chặng 6 (Colab) chưa thực hiện. Nhãn đã khoá trước, đúng thứ tự bắt buộc của
> `RUBRIC.md:25-26` — lịch sử commit là bằng chứng.

### Trả lời năm câu hỏi ở cuối notebook

1.
2.
3.
4.
5.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`.

**Ảnh `train_07`, người thứ 1, khớp `right_wrist`.** Người đàn ông áo đỏ cầm cán ô bằng tay
phải; cán ô và các ngón tay nắm quanh nó che đúng vùng tâm khớp cổ tay. Bằng chứng nhìn thấy
vẫn còn đủ: toàn bộ cẳng tay phải lộ rõ từ khuỷu xuống, và các đốt ngón tay cho biết bàn tay
kết thúc ở đâu — nên vị trí cổ tay suy ra được bằng cách kéo dài trục cẳng tay tới chỗ nó gặp
cán ô. Khớp này nằm hẳn trong khung hình (y ≈ 340, ảnh cao 640), nên nó **không** thoả điều
kiện `v = 0` vốn chỉ dành cho khớp ra ngoài mép ảnh. Vì vậy tôi chọn `v = 1` và **vẫn đặt chấm**
ở vị trí ước lượng. Nếu để `v = 0`, khớp này bị loại khỏi phép tính OKS và biến mất khỏi dữ liệu
train — model sẽ học rằng "tay đang cầm vật thì không có cổ tay".

Đối chiếu: cùng bức ảnh đó, hai `left_ankle` / `right_ankle` lại là `v = 0` thật, vì bàn chân bị
mép dưới ảnh cắt mất. Hai quyết định ngược nhau trong cùng một người là cách kiểm tra nhanh xem
mình có đang hiểu đúng ranh giới giữa "bị che" và "ra ngoài khung" hay không.
