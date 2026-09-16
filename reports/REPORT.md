# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Tống Thành Danh (2A202602299)   Nhóm: SOLO   Ngày: 2026-09-16

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 314 / 148 / 31 |
| Thời gian trung bình mỗi ảnh | _(điền sau: tổng thời gian gán / 20)_ |

Tổng khớp: 493 = 17 × 29 — không skeleton nào bị xoá bớt điểm.
Trung bình 15.93 khớp có `v > 0` mỗi người (`outputs/visibility_report.json`).

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

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.810 | **0.850** |
| OKS@0.50 | 0.931 | **1.000** |
| OKS@0.75 | 0.724 | **0.897** |
| Lỗi `dao_trai_phai` | 2 | **0** |
| Lỗi `nham_nguoi` | 7 | **0** |
| Lỗi `xoa_khop_bi_che` | 2 | **0** |
| Lỗi `truot_han` | 4 | **0** |
| Lỗi `lech_nhe` | 45 | 37 |
| Mức | Đạt | **Xuất sắc** |

Người: gold 29 | ghép được 29 | thiếu 0 | thừa 0 — ở cả hai lần chạy.

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_13`, người #2 (nam áo vàng kem phía sau), **cả 8 cặp trái/phải**: tôi gán anh ta là
  *quay lưng đi ra xa* nên đặt chi trái ở bên trái ảnh. Phóng to 16× thì thấy rõ vùng da mặt ở
  x 130-146 — anh ta **quay mặt về camera**. Đổi lại toàn bộ 8 cặp + kéo `nose` từ (127,44) về
  (140,42). OKS người này: 0.288 → 0.716.
- `train_15`, người #1 (người đội nón đen bên trái), **cả 8 cặp trái/phải** + đặt lại
  `left_shoulder` (152,152), `right_shoulder` (72,158), `left_elbow` (162,180),
  `right_elbow` (55,185), `left_wrist` (150,207), `right_wrist` (88,210), `nose` (128,130).
  Nguyên nhân: tôi tưởng anh ta đứng nghiêng hẳn nên dồn hai tay về cùng một phía; thực tế anh
  ta hơi quay về camera, hai tay tách ra rõ (găng ở x≈88, tay trần ở x≈150). OKS: 0.442 → qua ngưỡng.
- `train_03`, người #1 và #2, `right_elbow` + `right_wrist`: **cánh tay cầm ly nước là của người
  #2 (áo da đen), không phải người #1 (áo xanh rêu)** — tay áo trong ảnh phóng to là da đen bóng,
  không phải vải xanh rêu. Chuyển `right_wrist` của #2 sang (218,148) và `right_elbow` sang
  (258,197); trả `right_wrist`/`right_elbow` của #1 về cánh tay buông xuống ở (252,228)/(228,205).
- `train_10`, người #1, `left_hip` + `right_hip`: từ `v=0` → `v=1` kèm chấm ước lượng, rồi dời
  sang trái (345,320) và (245,300) — thân người nằm vắt ngang xe kéo dài **sang trái** sau kính
  chắn gió, không phải sang phải như tôi đoán ban đầu. OKS: 0.667 → qua ngưỡng.
- `train_14`, người #1, `right_wrist`: kéo từ (240,280) về (226,268) cho khỏi rơi vào vùng da
  cánh tay của người #2.
- `train_04`, **người nữ** `left_wrist` → (415,340) (găng Fox to) và `right_wrist` → (352,415) `v=1`;
  **người áo hoodie** `left_wrist` → (297,418) (găng dưới-trái trên ghi-đông) và `right_wrist` → `v=0`.
  Phóng to 7× thấy trong ảnh có **hai găng tay riêng biệt**, tôi đã gộp nhầm thành một.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Hai ảnh: `train_13` (người #2) và `train_15` (người #1). **Cả hai đều là ảnh khó**, nhưng khó theo
hai kiểu khác nhau, và cả hai đều có chung một nguyên nhân gốc: **tôi suy ra hướng người từ bối
cảnh thay vì từ bằng chứng trên chính cơ thể họ.**

- `train_13` người #2 mờ vì nằm ngoài vùng lấy nét. Tôi thấy một bóng người đứng trên vỉa hè và
  mặc định "người đi đường thì đang đi ra xa" — một giả định về bối cảnh, không phải quan sát.
  Phóng to đủ lớn thì vùng da mặt hiện ra ngay.
- `train_15` người #1 bị xe mô tô che từ ngực trở xuống. Tôi thấy mặt anh ta hướng sang phải nên
  suy ra camera đứng bên phải người — nhưng hướng *mặt* không quyết định hướng *thân*.

Điều đáng chú ý: ở `train_02` tôi **không** sai, dù đó cũng là ảnh chụp sau lưng và
`check_pose_labels.py` còn cảnh báo nghi đảo trái/phải. Khác biệt là ở đó tôi đã đi tìm bằng chứng
vật lý trên chính bức ảnh (moay-ơ xe có cần gạt QR, không thấy líp ⇒ đang nhìn mặt trái xe) thay vì
đoán. Bài học rút ra: **khi không thấy mặt, phải tìm một vật bất đối xứng trong ảnh để neo chiều,
chứ không được suy từ "người này chắc đang đi đâu".**

Ghi chú: `tools/check_pose_labels.py` vẫn báo 1 cảnh báo nghi đảo trái/phải ở `train_02`. Sau khi
chấm với gold, `train_02` **không** có lỗi `dao_trai_phai` — xác nhận cảnh báo đó là dương tính giả
của heuristic (nó giả định mặt quay về phía camera).

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
