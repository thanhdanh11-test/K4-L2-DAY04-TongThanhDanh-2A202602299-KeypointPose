# Mini guideline - nhóm: SOLO  |  người gán: Tống Thành Danh (2A202602299)  |  ngày: 2026-09-16

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | **`v = 2`**, đặt chấm ở khớp háng ước lượng từ đường viền thân + đường cạp quần. Chỉ hạ xuống `v = 1` khi có **vật khác** (yên xe, bàn, người khác) chắn vùng hông. | Quần áo không phải vật che. Nếu coi quần áo là che thì mọi người mặc đồ đều `v=1`, cờ mất hết ý nghĩa phân biệt. Đây cũng là quy ước của COCO nên bảng đếm còn so được với gold. Ví dụ áp dụng: `train_05` (hông `v=2`, quần dài) vs `train_03` người #1 (hông phải `v=1` vì **yên xe đạp** che). |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Còn thấy vành tai hoặc dái tai -> `v = 2`. Bị tóc/mũ phủ kín, chỉ suy ra được từ đường viền đầu -> `v = 1`, vẫn đặt chấm ở ngang đuôi mắt, cách mép đầu ~1/2 bán kính đầu. | Cờ mô tả **bằng chứng nhìn thấy**, không mô tả vật che. Ví dụ: `train_01` người #2 tai trái thấy rõ cả dái tai + khuyên -> `v=2`; `train_01` người #1 tóc xoăn phủ kín hai tai -> `v=1`; `train_04` cả hai người đội mũ fullface -> hai tai `v=1`. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Khớp nằm **ngoài** mép ảnh -> `v = 0`, toạ độ để (0,0). Không kéo chấm về nằm trên mép ảnh. | `v=0` là "ra ngoài khung", đúng định nghĩa duy nhất rubric cho phép. Kéo chấm lên mép ảnh tạo một khớp giả ở vị trí sai, model học được rằng đầu gối luôn nằm ở đáy ảnh. Ví dụ: `train_01` cả hai người (gối + cổ chân dưới mép dưới), `train_07` (hai cổ chân), `train_04` người #1 (cổ tay phải ra ngoài mép **trái**). |
| Cổ tay nằm sau tay lái / sau thân mình | Vật cầm tay che **tâm khớp** nhưng vẫn thấy cẳng tay -> `v = 1`, đặt chấm theo hướng cẳng tay, ngay chỗ cẳng tay chạm vật. Không thấy gì của cánh tay đó -> vẫn `v = 1` (còn trong khung), ước lượng từ vai + tư thế. | Đúng ca mẫu của `lab-guide.html` (train_07, right_wrist, "vật cầm tay che một phần tâm khớp"). Ví dụ: `train_07` cổ tay phải cầm cán ô -> `v=1`; `train_06` toàn bộ tay phải (phía xa) khuất sau thân -> `v=1`. |
| Hai người chồng lên nhau | Gán **xong hẳn một người rồi mới sang người kế**. Khớp của người sau bị người trước che -> `v = 1`, đặt ở vị trí ước lượng **trên cơ thể người sau**, tuyệt đối không kéo sang đường viền người trước. | Đây là cách duy nhất tránh lỗi `nham_nguoi` - lỗi số 2 của slide 46. Ví dụ: `train_03` người #1 có vai trái + khuỷu trái bị người #2 (áo da) che -> `v=1`. |
| Người nhỏ đến mức nào thì không gán nữa | Bộ 20 ảnh này mọi người đều đủ lớn nên **gán tất cả**. Không gán: người **in trên ảnh/biển quảng cáo**, người trên màn hình, tượng, búp bê. | Nhãn phải mô tả người thật trong cảnh; ảnh in là hoạ tiết của một vật thể khác. Ví dụ đã gặp: `train_08` có các khuôn mặt in trên biển "LEARNER MODELS" -> **không gán**; `train_03` có một con búp bê dưới đất -> **không gán**. |

### Ảnh mẫu kiểm chứng cho sáu luật

Các ảnh dưới đây được xuất bằng `tools/visualize_pose.py` từ chính nhãn đã khoá. Màu xanh/cam
phân biệt hai bên cơ thể; điểm vàng là khớp `v=1`. Chúng giữ nguyên vai trò bằng chứng trực quan
của screenshot CVAT nhưng có thêm đường nối để dễ phát hiện nhầm người và đảo trái/phải.

| Luật | Ảnh kiểm chứng |
| --- | --- |
| Hông dưới quần áo dài | ![`train_05`: hông vẫn `v=2`](outputs/vis_train/train_05.jpg) |
| Tai bị tóc/mũ che | ![`train_04`: tai dưới mũ là `v=1`](outputs/vis_train/train_04.jpg) |
| Người bị cắt ở mép ảnh | ![`train_01`: gối và cổ chân ngoài khung là `v=0`](outputs/vis_train/train_01.jpg) |
| Cổ tay sau vật/thân | ![`train_07`: cổ tay cầm cán ô là `v=1`](outputs/vis_train/train_07.jpg) |
| Hai người chồng lên nhau | ![`train_03`: mỗi skeleton bám đúng một người`](outputs/vis_train/train_03.jpg) |
| Người thật so với ảnh in/búp bê | ![`train_08`: chỉ người thật trên xe được gán`](outputs/vis_train/train_08.jpg) |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_02`, người thứ `1`, khớp `left_shoulder / right_shoulder` (và cả 5 khớp mặt)

- Mơ hồ ở chỗ nào: người đạp xe **quay lưng**, chỉ thấy gáy và ba lô. Không có mắt/mũi/tai
  nào nhìn thấy được, và không thể biết vai nào là vai trái nếu chỉ nhìn đường viền áo.
  `check_pose_labels.py` báo "left_shoulder/right_shoulder nằm ngược chiều so với hai mắt".
- Bạn quyết thế nào: dùng **bằng chứng cơ khí** thay vì đoán. Moay-ơ sau có **cần gạt QR**
  và **không thấy líp/cassette** -> đang nhìn **mặt trái (non-drive side)** của xe -> camera
  đứng bên **trái** người. Thêm một kiểm chứng che khuất: cánh tay có găng trắng vẽ **đè lên**
  áo -> nó gần camera nhất -> đó là tay **trái**. Kết luận: chi bên trái nằm ở x **lớn hơn**.
  Năm khớp mặt để `v = 1`, đặt theo hình học đầu (mũi ở phía trước nhất, tai phải ở sau gáy).
- Vì sao: nhìn sau gáy thì mắt **đảo chiều** so với vai - đó là hình học đúng, không phải lỗi.
  Heuristic trong `check_pose_labels.py` giả định mặt quay về phía camera nên báo dương tính giả.
- Nếu người khác quyết ngược lại thì model học sai cái gì: học rằng "chi ở phía x lớn = chi phải".
  Với `fliplr=0.5` trong `data.yaml`, augmentation lật ảnh dạy cái sai này **hai lần** - một lần
  ở ảnh gốc, một lần ở ảnh đã lật. Đó chính là slide 15.

### Ca 2 - ảnh `train_01`, người thứ `1` và `2`, khớp `left_knee / right_knee / left_ankle / right_ankle`

- Mơ hồ ở chỗ nào: hai người bị **cắt ngang hông** ở mép dưới ảnh (ảnh cao 427px, hông ở y≈377).
  Vẫn "suy ra được" đầu gối nằm ở đâu, nên có thể lập luận là `v = 1` (bị che, vẫn đặt chấm).
- Bạn quyết thế nào: `v = 0`, **không** đặt chấm. Tính ra đầu gối rơi vào y≈480 > 427.
- Vì sao: rubric ghi thẳng `v = 0` **chỉ** dùng cho khớp ra ngoài khung, và đây đúng là ra ngoài
  khung chứ không phải bị che. `check_pose_labels.py` vẫn cảnh báo "4 khớp v=0 trong khi người nằm
  gọn giữa ảnh" - dương tính giả, vì bbox do CVAT tính chỉ từ các điểm **có chấm** nên nó dừng ở
  hông và không chạm mép dưới.
- Nếu người khác quyết ngược lại thì model học sai cái gì: nếu gắn `v=1` và kéo chấm lên nằm trên
  mép ảnh, model học rằng "đầu gối luôn nằm ở đáy ảnh" - nó sẽ đoán ra đầu gối giả ở mọi ảnh
  bị crop ngang thân.

### Ca 3 - ảnh `train_07`, người thứ `1`, khớp `right_wrist`

- Mơ hồ ở chỗ nào: bàn tay nắm **cán ô**, cán ô che đúng tâm khớp cổ tay. Thấy rõ cẳng tay và
  các ngón tay, nhưng chỗ chính xác của cổ tay thì bị vật cầm che.
- Bạn quyết thế nào: `v = 1`, đặt chấm **theo hướng cẳng tay**, tại điểm cẳng tay gặp cán ô.
- Vì sao: đây là đúng định nghĩa "bị che nhưng vẫn suy ra được vị trí hợp lý từ phần cơ thể
  xung quanh". `lab-guide.html` dùng chính ca này (`train_07` / `right_wrist` / "vật cầm tay che
  một phần tâm khớp") làm ví dụ mẫu.
- Nếu người khác quyết ngược lại thì model học sai cái gì: nếu để `v = 0`, khớp này bị **loại khỏi
  phép tính OKS** và biến mất khỏi dữ liệu train -> model học rằng "tay đang cầm vật thì không có
  cổ tay", và sẽ bỏ sót cổ tay ở mọi ảnh người cầm đồ.

### Ca 4 (bổ sung) - ảnh `train_08`, khớp: cả bộ - có gán những khuôn mặt in trên biển quảng cáo không?

- Mơ hồ ở chỗ nào: biển "LEARNER MODELS" phía sau có in **ảnh chân dung người thật**, đủ lớn để
  đặt được keypoint.
- Bạn quyết thế nào: **không gán**. Chỉ gán người đang ngồi trên xe.
- Vì sao: người in trên biển là hoạ tiết bề mặt của một vật thể khác, không phải người trong cảnh.
  Cùng lý do đó, con búp bê dưới đất ở `train_03` cũng không gán.
- Nếu người khác quyết ngược lại thì model học sai cái gì: model học rằng poster, màn hình, tranh
  tường đều là "người" -> sinh ra rất nhiều false positive ở ảnh đường phố và cửa hàng.

## 4. Visibility report cá nhân (không kiểm chéo)

- **Không thực hiện kiểm chéo** theo phạm vi bài làm cá nhân; bỏ qua phần so sánh với bạn cùng
  lớp/cùng nhóm. Không có số liệu của người thứ hai nên không kết luận khớp nào lệch nhất và
  không bổ sung luật giả dưới danh nghĩa “đã thống nhất”.

### Bảng đếm của riêng tôi

20 ảnh, 29 skeleton, 493 khớp. Tổng `v=2` 314 | `v=1` 148 | `v=0` 31 → **`%v=1` toàn bài = 30%**.

Ba khớp `%v=1` cao nhất: `right_ear` 83%, `left_ear` 76%, hai mắt 34%.
Ba khớp `%v=1` thấp nhất: `left_shoulder` 3%, `right_shoulder` 10%, `left_elbow` 14%.

Khi đọc chênh lệch với gold, `%v=1` của tôi có thể **cao hơn gold** ở nhiều khớp, vì luật lớp bắt
đặt chấm cho khớp bị che (`v=1`) trong khi COCO dùng `v=0` cho cả "bị che" lẫn "không gán nhãn".
`README.md` mục cuối nói rõ đây không phải lỗi. Không dùng gold thay cho một bài kiểm chéo vì hai
nguồn áp dụng quy ước visibility khác nhau.
