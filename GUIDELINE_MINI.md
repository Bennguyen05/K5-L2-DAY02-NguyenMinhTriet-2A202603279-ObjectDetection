# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Xe con chở người, 4-7 chỗ, thân kín | Sedan, hatchback, SUV/crossover, coupe, taxi|
| `truck` |Xe chở hàng có cabin tách biệt với thùng hàng, hoặc có thùng đặc biệt. |Xe tải các cỡ, xe ben, xe bồn, đầu kéo |
| `bus` |Xe chở khách cỡ lớn, thân dài, nhiều cửa sổ dọc thân, trên 16 chỗ. |Xe buýt thành phố, xe khách đường dài, xe đưa đón công ty cỡ lớn. |
| `van` |Xe thân hộp liền khối, chở hàng nhẹ hoặc 7-16 khách. |Dáng hộp, mui cao, thường ít cửa sổ bên hông |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: hình chữ nhật nhỏ nhất bao trọn phần thân xe nhìn thấy được, tiếp xúc với điểm ngoài cùng ở cả bốn phía.
- Vật thể bị che khuất: vẽ box ôm phần nhìn thấy được, không đoán phần bị che, trường hợp vật có thể nhìn thấy dưới 20% thì không gán nhãn, nếu khoảng 20-30% thì đưa vào edge case để xem xét schema hoặc hỏi Lab Coach 
- Vật thể bị cắt bởi rìa ảnh: vẽ box tới sát rìa ảnh, ôm phần nhìn thấy, trường hợp vật có thể nhìn thấy dưới 20% thì không gán nhãn, nếu khoảng 20-30% thì đưa vào edge case để xem xét schema hoặc hỏi Lab Coach 
- Vật thể rất nhỏ/mơ hồ: nếu box quá nhỏ không xác định được lớp thì không gán nhãn. Khi phân vân giữa hai lớp, chọn lớp theo đặc điểm hình dáng rõ nhất. Ghi lại các edge case để xem xét schema hoặc hỏi Lab Coach

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` |Một car chen trước một xe van, xe còn lại bị cột đèn che một phần|Không gán |Hai xe chồng nhau thì mỗi xe một box trên phần nhìn thấy. Riêng xe van chỉ còn lộ một phần nhỏ, không chắc là van, car hay bus nên không tự đoán, ghi edge case và hỏi Lab Coach |
| `drive_006` |Góc dưới bên phải chỉ lộ một đoạn ngắn của xe, bị rìa ảnh cắt | Không gán|Chỉ còn mảnh rất nhỏ, không xác định chắc lớp thì không tự đoán, ghi vào mini guideline và hỏi. Nếu thấy rõ lớp thì box phải chạm đúng rìa ảnh |
| `drive_010` |Xe ở góc trên bên phải quá xa, quá nhỏ để nhận biết|Không gán |Không xác định chắc được lớp thì không tự đoán, ghi lại để hỏi. |
