# BÀI TẬP LỚN HỆ QUẢN TRỊ CƠ SỞ DỮ LIỆU
# ĐỀ TÀI:  THIẾT KẾ VÀ CÀI ĐẶT CSDL QUẢN LÝ CẦM ĐỒ
## Sinh Viên: Phạm Trung Hiếu
## Lớp: K58KTP
## MSSV: K225480106084
## GVHD: Ths.Đỗ Duy Cốp
**4.1. Kiểm thử Event 1: Tiếp nhận và Đăng ký Hợp đồng Mới**
<img width="733" height="412" alt="image" src="https://github.com/user-attachments/assets/4c3efdb4-441b-4192-b68d-a05b3528be29" />

<img width="732" height="256" alt="image" src="https://github.com/user-attachments/assets/58ba4fe3-db25-41e5-9081-6a6411199cd9" />

**4.2. Kiểm thử Event 2: Tính toán công nợ lãi đơn và lãi kép thời gian thực**
4.2.1: Kiểm thử chức năng tính Lãi đơn (Hợp đồng trong hạn)
Bối cảnh phân tích: Hợp đồng số 1 (MaHD = 1) giải ngân số tiền gốc 15.000.000đ vào ngày 01/06/2026. Mốc hết hạn lãi đơn là ngày 01/07/2026. Tính đến ngày chạy kiểm thử (15/06/2026), hợp đồng này đã trải qua đúng 14 ngày vay và nằm hoàn toàn trong mốc tính lãi đơn (0.5%/ngày).
<img width="732" height="353" alt="image" src="https://github.com/user-attachments/assets/40cbed38-c038-4f9a-ad7b-c4901884fdac" />

**4.2.2: Kiểm thử chức năng tính Lãi kép dồn gốc (Hợp đồng quá hạn)**
Bối cảnh phân tích: Hợp đồng số 2 (MaHD = 2) giải ngân số tiền gốc 8.000.000đ vào ngày 01/05/2026. Mốc Deadline 1 hết hạn lãi đơn là ngày 31/05/2026 (vừa vặn 30 ngày tính lãi đơn). Tính đến ngày chạy kiểm thử (15/06/2026), hợp đồng này đã quá hạn Deadline 1 được 15 ngày và hệ thống phải tự động kích hoạt vòng lặp tính lãi kép lũy tiến dồn gốc theo từng ngày quá hạn.
<img width="732" height="350" alt="image" src="https://github.com/user-attachments/assets/352e062a-f827-4053-9080-9752a6c1ff0e" />

**4.3. Kiểm thử Event 3: Xử lý trả nợ và Đề xuất hoàn trả tài sản**
Mục tiêu: Xác thực chức năng của Store Procedure sp_ProcessPayment trong việc tự động tính toán dư nợ tại thời điểm khách đến đóng tiền, khấu trừ công nợ, tự động cập nhật trạng thái hợp đồng (TrangThai), lưu vết dòng tiền mặt vào nhật ký kiểm toán hệ thống (LogThanhToan), đồng thời kiểm thử thuật toán thông minh tự động đề xuất danh sách tài sản khách được phép rút về trước.
4.3.1: Khách hàng thanh toán một phần (Trả góp) và hệ thống đề xuất trả đồ
Bối cảnh phân tích: Để đảm bảo tính minh bạch và cô lập dữ liệu kiểm thử, ta tiến hành thử nghiệm trên một hợp đồng mới tinh vừa được tiếp nhận trong ngày hôm nay (15/06/2026).
- Khách hàng giải ngân vay gốc số tiền: 20.000.000đ.
- Thế chấp đồng thời 02 tài sản độc lập: 01 Máy tính Laptop (định giá 25.000.000đ) và 01 Máy ảnh Mirrorless (định giá 18.000.000đ) --> Tổng giá trị tài sản tiệm cầm đồ đang giữ là 43.000.000đ.
- Ngay sau khi lập hợp đồng, khách hàng xin nộp trả trước ngay lập tức số tiền 12.000.000đ để giảm tải áp lực nợ gốc và xin rút bớt món đồ có giá trị thấp về trước.
<img width="620" height="271" alt="image" src="https://github.com/user-attachments/assets/5c9d62e9-dea2-4433-bfc8-acff92962423" />


<img width="733" height="458" alt="image" src="https://github.com/user-attachments/assets/02273db9-c814-4281-b32e-bef79ade668a" />

<img width="722" height="91" alt="image" src="https://github.com/user-attachments/assets/f73145a8-eba1-4b85-baa7-926c4c17cf20" />


4.3.2: Kiểm thử tính năng chặn giao dịch bảo vệ (Tài sản đã bị thanh lý bán đứt)
Bối cảnh phân tích: Dựa vào kịch bản nạp dữ liệu mẫu ở đầu chương, Hợp đồng số 7 (MaHD = 7) đã bị cưỡng chế thanh lý, tài sản thuộc hợp đồng này đã bị tiệm đem bán ra thị trường để thu hồi vốn gốc và hệ thống đã khóa cờ an toàn IsSold = 1. Ta tiến hành giả lập tình huống nhân viên vô tình chọn nhầm mã hợp đồng này để thu tiền đóng nợ của khách xem hệ thống có ngăn chặn kịp thời hay không.
<img width="732" height="361" alt="image" src="https://github.com/user-attachments/assets/d47b2530-1aa7-42c4-aa1d-f34bcbd6e860" />

**4.4. Kiểm thử Event 4: Truy vấn danh sách nợ xấu**
Mục tiêu: Kiểm tra khả năng trích xuất dữ liệu và phân loại nợ xấu tự động của hệ thống. Câu lệnh này sử dụng trực tiếp hàm tài chính dbo.fn_CalcMoneyContract để quét thời gian thực toàn bộ các hợp đồng đã vượt quá mốc Deadline1, tính toán chính xác số ngày quá hạn, tổng số tiền nợ hiện tại (bao gồm cả lãi kép dồn gốc) và đưa ra viễn cảnh dự báo công nợ sau đó 30 ngày để chủ tiệm chủ động lên phương án thu hồi vốn.
Mốc thời gian hệ thống giả định để đối soát: Ngày 15/06/2026.
<img width="731" height="268" alt="image" src="https://github.com/user-attachments/assets/bb06430d-40c8-4f9a-a23b-f98cfa9874d3" />

**4.5. Kiểm thử Event 5: Hoạt động tự động hóa của hệ thống Triggers**
4.5.1. Kiểm thử kịch bản Trigger 1: Tự động chuyển hợp đồng sang "Quá hạn (nợ xấu)"
<img width="732" height="397" alt="image" src="https://github.com/user-attachments/assets/3117f5e8-6dd8-4ed9-b9fe-3de5be288b5a" />

4.5.2. Kiểm thử kịch bản Trigger 2: Tự động chuyển tài sản sang "Sẵn sàng thanh lý"

<img width="733" height="344" alt="image" src="https://github.com/user-attachments/assets/e846029c-2110-421b-a82c-26a005d69fc7" />


4.5.3. Kiểm thử kịch bản Trigger 3: Đóng băng kho và chuyển tài sản sang "Đã bán thanh lý"
<img width="732" height="353" alt="image" src="https://github.com/user-attachments/assets/29090738-7b8c-4944-8e5b-28487bbc1c4d" />

