# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Minh Triết
- MSSV: 2A202603279
- Email: benny.nee0512@gmail.com
- Lớp: 2A
- Link repo bài làm: https://github.com/Bennguyen05/K5-L2-DAY02-NguyenMinhTriet-2A202603279-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1Id5tBWMXg8phKa4REuk0NEJu5E4q0iSs?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: ĐẠT cấu trúc: 50/50 file nhãn hợp lệ
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_006, drive_008, drive_009, drive_010, drive_011, drive_012, drive_014, drive_016, drive_017
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: bỏ sót các trường hợp vẫn nhìn ra được class nhưng hình nhỏ, các trường hợp bị che khuất nhưng vẫn xác định được box/class và các ảnh bị mờ. Cách sửa là xác định lại schema và vẽ box/class tương ứng. 
- Commit khóa nhãn ban đầu: 83ebc73 Final train labels, remove wrong lable from eval

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |346 |401 |
| FP |127 |228 |
| FN |307 |252 |
| Precision |0.7315 |0.6375 |
| Recall |0.5299 |0.6141 |
| F1 |0.6146 |0.6256 |
| mean_iou_of_matches |0.8671 |0.8479 |
| passed |false |false |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: CPU (Intel Xeon 2.20GHz), 50 epoch, 0.566 giờ. Ultralytics 8.4.208, Torch 2.14.1+cpu, Python 3.13.18. 
- Các tham số thay đổi so với notebook nếu có và lý do: không thay đổi
- Đường dẫn best checkpoint trong phiên chạy:/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.451 |
| Recall |0.246 |
| mAP50 |0.276 |
| mAP50–95 |0.200 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook (đã trả lời trong notebook):

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Mở CVAT |docker compose không kết nối được Docker API |Khởi động Docker Desktop, chạy lại docker compose start |CVAT chạy ở localhost:8080 |
| Self-QC | tools/check_yolo_labels.py không tìm thấy | Đang đứng nhầm thư mục cvat-day2; chuyển sang thư mục repo | Chạy được |
| Gold | upload nhầm gold train vào dataset/labels/eval/ trên GitHub (commit 61c99ed) | Chuyển gold sang gold_train_labels/, xóa khỏi eval, thêm gold_train_labels/ vào .gitignore | Gold còn trong lịch sử commit cũ |
| Nhãn |drive_001.txt của train bị xóa nhầm (commit a53771c) | Khôi phục từ commit trước đó | Đủ 50 file|
| Colab | Train chạy trên CPU, mất 34 phút | Giữ nguyên kết quả | Không tối ưu được thời gian; chưa train lại|

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo
Công cụ AI: Claude (Anthropic), dùng để hướng dẫn thao tác Docker/CVAT/git/Colab, đọc output đánh giá. Phần tự kiểm tra gồm báo cáo, các câu lệnh trên Ubuntu, Colab, Github, gán nhãn và kiểm tra trên CVAT, commit và push lên Github. 

