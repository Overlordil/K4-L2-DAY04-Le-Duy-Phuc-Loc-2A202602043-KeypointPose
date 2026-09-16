# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Duy Phúc Lộc   Nhóm: ______   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 27 |
| v=2 / v=1 / v=0 | 296 / 134 / 29 |
| Thời gian trung bình mỗi ảnh | 3m |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear — 72% 
2. right_ear — 48% 
3. right_hip — 38% 

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích. Đúng là những khớp tôi thấy khó gán nhất

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.967 | 0.962 |
| OKS@0.50 | 0.931 | 1.000 |
| OKS@0.75 | 0.931 | 1.000 |
| Lỗi `dao_trai_phai` | | |
| Lỗi `nham_nguoi` | | |
| Lỗi `xoa_khop_bi_che` | | |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa lại một số lỗi”. -->

- Ảnh train_13 / người thứ 2 và thứ 3 / box 2 người thiếu

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

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
| pose_mAP50 | 0.845 | 0.845 | 0 |
| pose_mAP50-95 | 0.6853 | 0.6908 | 0.0055 |
| pose_precision | 0.9734 | 0.9792 | 0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0 |
| box_mAP50-95 | 0.9785 | 0.96 | 0.0185 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

pose_mAP50-95 thay đổi bao nhiêu sau fine-tune?
Baseline: 0.6853
Fine-tuned: 0.6908
Thay đổi: +0.0055 = +0.55 điểm phần trăm

Vậy pose_mAP50-95 tăng nhẹ, không giảm.

Điều này cho thấy 20 ảnh của bạn đã giúp model học thêm một chút về đặc điểm pose trong chính tập ảnh của bạn — chẳng hạn cách người xuất hiện, tư thế, góc nhìn hoặc đặc điểm hình ảnh cụ thể của dataset. Tuy nhiên, mức tăng chỉ 0.0055, nên ảnh hưởng của fine-tune là khá nhỏ.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?

Nếu lấy mAP50-95:

| Metric | Baseline | Fine-tuned |
| --- | ---: | ---: |
| Box mAP50-95 | 0.8119 | 0.8041 |
| Pose mAP50-95 | 0.6853 | 0.6908 |
| Box − Pose | 0.1266 | 0.1133 |

Tức là:

Baseline: box cao hơn pose 0.1266 → 12.66 điểm %
Fine-tuned: box cao hơn pose 0.1133 → 11.33 điểm %

=> Model tìm người bằng bounding box dễ/chính xác hơn tìm chính xác các khớp.
3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

test_03 - lệch nhẹ

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

Ảnh có OKS thấp nhất là train_13, với OKS = 0.595. Điều này cho thấy pose của model và annotation của tôi có sự khác biệt lớn nhất ở ảnh này. Sau khi kiểm tra trực quan ảnh và đối chiếu với vị trí thực tế của các khớp trên người, annotation của tôi là đúng, còn model dự đoán sai. Do đó, OKS thấp ở train_13 chủ yếu phản ánh lỗi dự đoán keypoint của model.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?

Có, ảnh train_13 là ảnh có OKS thấp nhất ở cả hai phép so sánh. Khi so sánh annotation của tôi với gold, OKS thấp nhất là 0.8912 ở người thứ hai trong train_13. Khi so sánh model với annotation của tôi, train_13 cũng có OKS thấp nhất là 0.595. Điều này cho thấy train_13 là một ảnh tương đối khó, đặc biệt do có nhiều người và các pose có thể gây khó khăn cho việc dự đoán keypoint. Tuy nhiên, annotation của tôi vẫn khá sát gold và không có người bị thiếu hoặc thừa. Vì vậy, kết quả này chủ yếu cho thấy đây là trường hợp model khó dự đoán chính xác, thay vì chứng minh annotation của tôi bị sai nghiêm trọng.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người, khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Trong ảnh train_13, người thứ nhất, tôi phải quyết định trạng thái của right_wrist giữa v=1 và v=0. Tôi dựa vào phần cẳng tay và bàn tay vẫn còn nhìn thấy trong ảnh, đồng thời vị trí cổ tay có thể được xác định từ phần cơ thể liền kề. Vì khớp vẫn còn nằm trong vùng ảnh và có đủ bằng chứng thị giác để xác định vị trí, tôi chọn v=1 thay vì v=0. Kết quả đối chiếu cho thấy điểm này chỉ lệch nhẹ khoảng 14 px so với gold, nên quyết định giữ keypoint này là có cơ sở.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
