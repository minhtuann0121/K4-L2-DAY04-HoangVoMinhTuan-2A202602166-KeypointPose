# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Hoàng Võ Minh Tuấn   Nhóm: T020   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 356 / 102 / 35 |
| Thời gian trung bình mỗi ảnh | 1 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` - 41% (12/29)
2. `right_ear` - 38% (11/29)
3. `right_eye` - 31% (9/29)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng 3 khớp đó rất khó. Vì nó gần với nhau nên dễ lẫn lộn.VÍ dụ ở tấm này, vì người này quay đầu nên mắt, mũi, tai đều không thấy và vì ko thấy nên dễ gán nhãn lẫn lộn vị trí. ![alt text](train_02.jpg)

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9398 | 0.9398 |
| OKS@0.50 | 1.0 | 1.0 |
| OKS@0.75 | 1.0 | 1.0 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |
| Lỗi `lech_nhe` (tham khảo, không trừ điểm) | 9 | 9 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

-
-
-

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.0 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

Số liệu: `box_mAP50-95` 0.8041 vs `pose_mAP50-95` 0.6908 → chênh **0.1133**; ở ngưỡng 50:
   `box_mAP50` 0.96 vs `pose_mAP50` 0.845 → chênh **0.115**. Theo hai cặp số này, model tìm
   *người* (box) dễ hơn tìm *khớp* (pose). **Vì sao:**

   - **Số ràng buộc và dung sai khác nhau.** Box chỉ cần một khung bao đúng, dung sai rất rộng:
     với người trung vị trong 20 ảnh (diện tích box 30 569 px², tương đương hình vuông cạnh
     175 px), khung **dịch 58 px vẫn còn IoU 0.5**. Pose thì chấm từng khớp bằng bán kính
     `2·sigma·sqrt(area)` (`tools/poselib.py` dòng 133): cùng người đó dung sai chỉ là
     **8.7 px ở mắt, 9.1 px ở mũi, 12.2 px ở tai, 21.7 px ở cổ tay** (rộng nhất là hông 37.4 px).
     Tức đầu pose phải chính xác gấp ~5-7 lần ở vùng mặt, và phải đúng **cả 17 điểm** cho một
     người chứ không phải một khung. (Số tính bằng `tools/poselib.py` trên
     `dataset/labels/train` - 29 skeleton.)
   - **Khớp bị che thì phải đoán.** Trong 493 chỗ khớp (29 người × 17), nhãn của lớp để 35 khớp
     `v=0` và gắn **102 khớp `v=1`** - 22% số khớp được chấm là vị trí ước lượng giải phẫu, không
     nhìn thấy. Box không bị ảnh hưởng bởi việc khớp bị che, nên phần khó này chỉ đè lên pose.
   - **Bằng chứng trên chính output mục 6:** nhiều ảnh model **đếm đúng số người** mà OKS vẫn
     thấp - `train_11` 0.73, `train_14` 0.742, `train_15` 0.765, `train_19` 0.77 (bốn ảnh này
     không có dòng "số người lệch"); ngược lại chỉ 2 ảnh model lệch số người (`train_03`,
     `train_10`). Vậy lỗi tập trung ở **toạ độ khớp**, không ở việc tìm người - khớp với khoảng
     cách 0.113 giữa hai chỉ số.
   - **Chênh ổn định ở cả hai ngưỡng** (0.115 tại mAP50, 0.1133 tại mAP50-95) → không phải hiện
     tượng của một ngưỡng, mà là độ khó hệ thống của hai bài toán.
   - **Chiều ngược lại vẫn có lỗi:** box không hoàn hảo - `train_10` model thấy 2 người (tôi và
     gold gán 1), `train_03` model thấy 4 (tôi và gold gán 2) → đầu detect **thừa người** ở 2/20
     ảnh, đúng kiểu sai mà `box_mAP50` vẫn để lọt vì IoU rộng.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   - Số liệu (cell chẩn đoán dùng đúng hàm `diagnose_pair` của repo, so model với **nhãn test
     phát sẵn**): `test_03` model đếm **đúng** số người (2/2) nhưng OKS chỉ **0.251**
     (model#2 ↔ nhãn#1) và **0.188** (model#1 ↔ nhãn#2) - thấp hơn hẳn 28 cặp còn lại của 10 ảnh
     test (các cặp kia 0.896-0.969).
   - **Tên lỗi:** `dao_trai_phai` trên **cả hai** skeleton - "đổi lại toàn bộ cặp trái/phải cho
     skeleton này thì OKS tăng hẳn". Đây là lỗi nguy hiểm nhất theo `RUBRIC.md` (augmentation lật
     ảnh dạy cái sai đó hai lần), và trong 10 ảnh test **chỉ `test_03`** mắc lỗi này.
   - **Kèm `truot_han`** (lệch > 3 lần bán kính dung sai) đúng ở nhóm khớp tay: `left_elbow`
     112 px (3.6×), `right_elbow` 107 px (3.4×), `left_wrist` 89 px (3.3×), `right_wrist` 94 px
     (3.5×); các khớp thân và chân chỉ `lech_nhe` (1.3-3.0×). Tức model đặt hai tay **sai bên** -
     khớp với kết luận đảo trái/phải, và giải thích luôn vì sao `pose_mAP50` (0.845) thấp hơn
     `box_mAP50` (0.96): một người bị đảo trái/phải làm hỏng gần hết 17 khớp, trong khi box của
     người đó vẫn đúng.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
**Trả lời:** Ảnh thấp nhất là **`train_06` - OKS 0.639** (dòng thấp nhất trong 29 dòng output
   mục 6; hai dòng kế tiếp là `train_11` 0.73 và `train_14` 0.742). **Nhãn của tôi đúng, model
   sai**, dựa vào hai bằng chứng:

   - Chính nhãn `train_06` của tôi chấm với gold được **0.9572** (`outputs/eval_vs_gold.json`,
     cặp gold người 1 ↔ bài của tôi người 1) - gần như trùng gold, trong khi model chỉ đạt 0.639
     khi chấm với nhãn đó.
   - Danh sách lỗi của `train_06` **không có lỗi ưu tiên 1-4 nào**: 0 `dao_trai_phai`,
     0 `nham_nguoi`, 0 `xoa_khop_bi_che`; chỉ có 2 mục `co_khac_gold` (hai tai: tôi ghi `v=1`,
     gold ghi `v=2`) và 7 khớp gold để `v=0` (`nose`, `left_eye`, `right_eye`, `right_elbow`,
     `right_wrist`, `right_knee`, `right_ankle`). Tức người gán và gold đồng ý với nhau trên phần
     lớn khớp, model mới là bên lệch.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   **Trả lời: Không trùng.** Ảnh tôi gán tệ nhất theo gold là **`train_03` người 1 - OKS 0.7931**
   (mục 2 ở trên); ảnh model đoán tệ nhất theo nhãn của tôi là **`train_06` - OKS 0.639**.

   Chi tiết từ output mục 6: `train_03` chiếm 2 dòng (0.798 và 0.946) **và** có dòng
   "số người lệch: model 4 / bạn 2" - model thấy 4 người trong khi tôi và gold đều gán 2 người;
   `train_10` cũng lệch số người (model 2 / bạn 1). `train_06` thì chỉ có 1 người, model không
   lệch số người mà lệch toạ độ. Đọc từ số: hai bên lệch nhiều nhất ở hai kiểu ảnh khác nhau -
   ảnh của tôi là ảnh có **nhiều ứng viên "người" hơn số được gán nhãn** (model đếm 4, tôi và
   gold gán 2; nhiều khả năng có người nhỏ / ở xa trong nền), ảnh của model là ảnh có **nhiều
   phần cơ thể không gán được** (gold để `v=0` cho 7/17 khớp: mắt, khuỷu, cổ tay, gối, cổ chân
   phải). Riêng `train_03` thì cả hai bên đều quanh 0.79-0.80, tức nó khó với cả hai, nhưng
   "tệ nhất" của hai bên không rơi vào cùng một ảnh.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
