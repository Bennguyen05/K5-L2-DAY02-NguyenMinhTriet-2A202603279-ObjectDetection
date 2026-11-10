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

https://github.com/Bennguyen05/K5-L2-DAY02-NguyenMinhTriet-2A202603279-ObjectDetection/blob/main/outputs/annotation_evaluation.json

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
https://github.com/Bennguyen05/K5-L2-DAY02-NguyenMinhTriet-2A202603279-ObjectDetection/tree/main/report/individual/assets/eval_predictions/visuals
https://github.com/Bennguyen05/K5-L2-DAY02-NguyenMinhTriet-2A202603279-ObjectDetection/tree/main/report/individual/assets/yolo26_runs

Trả lời bốn câu hỏi cuối notebook (đã trả lời trong notebook):

1. Lớp nào recall thấp nhất? Ảnh nào minh họa? bus: P = 0, R = 0, mAP50 = 0.017 (2 instance trong 2 ảnh eval). Tiếp theo là van (R 0.083), truck (R 0.20), và car cao nhất (R 0.70). Ở ảnh drive_051, gold có 1 xe bus ở góc trên bên phải ảnh (khoảng x = 0.83, y = 0.13). Đó là một xe thân hộp màu trắng, cao, nằm sát mép phải và gần các xe khác bị cắt bởi rìa ảnh. Model không đặt box đúng lên xe này ở ngưỡng conf 0.25; các box car gần nhất lệch sang mép phải. Đây là một ca bỏ sót (FN) của lớp bus. Nguyên nhân có thể là xe nằm sát rìa, hình dạng thân hộp khó phân biệt giữa bus, van và truck, và lớp bus có rất ít mẫu train, kèm nhãn train của lớp này chưa chắc nhất quán.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box? Khi mAP50 cao mà mAP50-95 thấp, box đã chồng đúng lên xe nhưng không đủ khít. Có thể người gán nhãn đã đặt lệch cạnh hoặc dư/hụt vài pixel nên IoU rơi xuống dưới các ngưỡng cao như 0.75-0.95. Đây là lỗi về độ chính xác của box, không phải lỗi nhận diện.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét? Gold eval có 12 van và 2 bus, trong khi model dự đoán 13 bus và 9 van. bus có P/R = 0, van có R 0.083, có thể là do đã nhầm lẫn thứ tự ID của bus/van trong train set. truck: chỉ 16 box trong train, R 0.20, chủ yếu là thiếu dữ liệu. Có thể nhầm với van khi tiêu chí “cabin tách khỏi thùng” áp dụng chưa đủ. car: lỗi chủ yếu là bỏ sót.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Tập eval phải là dữ liệu model chưa từng học để đo khả năng tổng quát hóa. Nếu đưa vào train, model có thể ghi nhớ các ảnh đó, mAP tăng giả và không còn phản ánh chất lượng trên ảnh mới.

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

