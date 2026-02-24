# Thực trạng
- Chưa có phần mềm quản lý thiết bị vật tư vẫn đang sử dụng excel
- Khó kiểm soát các hao phí ẩn (hư hỏng, hết hạn, thất thoát, sử dụng lãng phí)
- Khó thống kê, tra cứu
- Dự báo nhu cầu mua hàng dựa trên cảm tính dẫn đến tình trạng lúc thiếu (ảnh hưởng công việc), lúc thừa (tồn đọng vốn)
- Nhập liệu mất nhiều thời gian 

# Mục tiêu
- Kiểm soát tồn kho realtime theo từng hạng mục sản phẩm (tồn kho khả dụng, tồn sắp hết hạn, sản phẩm đã hết hạn)
- Ngăn chặn đứt gãy chuỗi cung ứng vật tư: cảnh báo tồn < Min, dự báo thiếu hàng trước 1 tháng (tự đưa ra dự báo dựa trên nhu cầu sử dụng thực tế), đề xuất số lượng mua thêm
- Giảm tồn kho dư thừa, thất thoát: cảnh báo tồn > Max, phát hiện vật tư có mức tiêu thụ chậm
- Tối ưu quy trình nghiệp vụ giảm thời gian nhập liệu (thay vì nhập tay có thể OCR hóa đơn và tự nhập), tự động map tên sản phẩm theo quy chuẩn
- Dự báo nhu cầu vật tư theo tuần, theo tháng, 
- Phân tích nhu cầu theo xu hướng: top vật tư tăng, vật tư giảm, vật tư bất thường
- Quản trị và kiểm soát nội bộ: audit log mọi thay đổi, phân quyền theo đúng chức năng nhiệm vụ

# Quy trình xử lý

## Danh mục sản phẩm ##

| Mã SP  | Tên Sản Phẩm          | Hàm Lượng | ĐVT | Lô         | Hạn Dùng | Tồn Kho | Thực Kiểm | Lệch | 60 ngày | 30 ngày |
| ------ | --------------------- | --------- | --- | ---------- | -------- | ------- | --------- | ---- | ------- | ------- |
| ABH001 | ABHAYRAB              | 0,5 ML    | Lọ  | 18URAB023  | 28-02-21 | 5       | 5         | 0    | ✅       | ✅       |
| ADA001 | ADACEL 0,5ml          | 0.5ml     | Lọ  | C5282BB    | 08-01-19 | 612     | 612       | 0    | ❌       | ❌       |
| ADA001 | ADACEL 0,5ml          | 0.5ml     | Lọ  | C5282BB    | 01-08-19 | 0       | 0         | 0    | ❌       | ❌       |
| AVA001 | Avaxim 80U            | 0.5 ml    | Lọ  | N1K831V    | 01-10-19 | 10      | 10        | 0    | ❌       | ❌       |
| AVA001 | Avaxim 80U            | 0.5 ml    | Lọ  | N1L173V    | 31-10-19 | 16      | 16        | 0    | ❌       | ❌       |
| BCG001 | BCG 1ml               | 1ml       | Lọ  | 386-10-16  | 07-02-20 | 160     | 160       | 0    | ⚠️      | ✅       |
| CER001 | CERVARIX 0.5ML        | 0.5 ML    | Lọ  | AHPVA327AC | 31-12-19 | 80      | 80        | 0    | 🔴      | 🔴      |
| ENG001 | Engerix B 10mcg 0.5ml | 0.5 ml    | Lọ  | AHBVC662BF | 30-06-20 | 1800    | 1800      | 0    | ✅       | ✅       |
| ENG002 | Engerix B 20mcg 1ml   | 1 ml      | Lọ  | AHBVC636CD | 30-09-19 | 80      | 80        | 0    | ❌       | ❌       |
| ENG002 | Engerix B 20mcg 1ml   | 1 ml      | Lọ  | AHBVC653AK | 31-03-20 | 208     | 208       | 0    | ⚠️      | ✅       |
| EUV001 | Euvax 10mcg 0.5ml     | 0.5 ml    | Lọ  | UFA17017   | 09-10-20 | 700     | 700       | 0    | ✅       | ✅       |
| EUV001 | Euvax 10mcg 0.5ml     | 0.5 ml    | Lọ  | UFA17025   | 29-11-20 | 350     | 350       | 0    | ✅       | ✅       |
| EUV002 | Euvax 20mcg 1ml       | 1 ml      | Lọ  | UFX17007   | 19-11-20 | 1280    | 1280      | 0    | ✅       | ✅       |
| GAR001 | Gardasil 0.5ml        | 0.5 ml    | Lọ  | N036053    | 28-01-20 | 40      | 40        | 0    | 🔴      | 🔴      |
| HAV001 | Havax 0.5ml           | 0.5 ml    | Lọ  | AC-010118  | 31-12-19 | 750     | 750       | 0    | 🔴      | 🔴      |
| HEP001 | Hepavax 10mcg 0.5ml   | 0.5 ml    | Lọ  | 1433020.04 | 28-12-19 | 100     | 100       | 0    | 🔴      | 🔴      |
| HEX001 | Hexaxim               | 0,5ml     | Lọ  | P3H411V    | 31-03-20 | 2900    | 2900      | 0    | ⚠️      | ✅       |

** Quy ước cảnh báo **
- 🔴 Còn ≤ 30 ngày
- ⚠️ Còn ≤ 60 ngày
- ❌ Đã hết hạn
- ✅ Còn an toàn

## Nhập liệu thông minh ##
**1. Nhập tay theo form danh mục sản phẩm**
**2. OCR quét hóa đơn**
- Dùng điện thoại chụp ảnh phiếu xuất hóa đơn/Hóa đơn đỏ từ nhà cung cấp
- AI xử lý: Tự động "đọc" ảnh, trích xuất: Tên vật tư, Số lượng, Số lô, Hạn sử dụng và điền vào form trên Web.
- Check trùng lặp: Nếu nhập một lô hàng đã tồn tại mã SKU, hệ thống cảnh báo ngay lập tức để tránh nhập đúp.

## Xuất kho thông minh ##
**1. Xuất kho bằng cách nhập dữ liệu theo form**
**2. Quét mã QR**
- Mỗi vật tư hoặc khay đựng vật tư được dán mã QR.
- Khi lấy hàng, nhân viên dùng điện thoại/máy quét tít mã -> Nhập số lượng lấy -> Xong.
- Cảnh báo tại chỗ: Nếu nhân viên chọn xuất lô hàng có hạn sử dụng xa hơn, trong khi có lô cũ hơn cần dùng trước, Web App sẽ hiện Popup: "Cảnh báo: Còn lô hàng hết hạn ngày dd/mm/yyyy. Vui lòng xuất lô này trước (FEFO)."

## Kiểm kê định kỳ ##
- Hỗ trợ kiểm kê: App hiển thị danh sách cần kiểm. Nhân viên đi đến đâu nhập số thực tế đến đó trên điện thoại (thay vì in giấy rồi về nhập lại Excel).
- AI đối chiếu: Tự động tính lệch (Thực tế vs Lý thuyết) và đưa ra cảnh báo

## Quản trị thông minh ##
### Dashboard quản trị thông minh (AI Dashboard)
- Mục tiêu: Kiểm soát toàn diện, ra quyết định dựa trên dữ liệu, không phải làm báo cáo thủ công.
Thay vì nhìn các bảng tính khô khan, Admin sẽ có một "Trung tâm điều hành":

- Biểu đồ thời gian thực: Hiển thị tồn kho hiện tại, giá trị tồn kho, tỷ lệ hao hụt ngay khi User nhập liệu.

- AI Cảnh báo sớm (Predictive Alert):

Tính năng: AI phân tích tốc độ tiêu thụ hiện tại và dự báo: "Với tốc độ dùng hiện tại, mã hàng Bơm tiêm 5ml sẽ hết trong 4 ngày tới. Cần nhập hàng ngay."

Kiểm soát hạn dùng: Hiển thị danh sách các thuốc/vật tư còn hạn < 30 ngày, < 60 ngày trên màn hình chính (ưu tiên đẩy hàng cũ ra trước - FEFO).

- AI Phát hiện bất thường (Anomaly Detection):

Tính năng: Hệ thống tự động so sánh dữ liệu lịch sử. Nếu một ngày/tuần nào đó lượng tiêu hao tăng đột biến (ví dụ: găng tay y tế tăng 300% so với trung bình), AI sẽ khoanh đỏ và gửi thông báo: "Cảnh báo: Tiêu hao bất thường tại Khoa Nội Tim Mạch".
### Quản lý đặt hàng và dự báo ###
- Đề xuất đơn hàng tự động: AI tính toán dựa trên (Tồn kho hiện tại - Mức an toàn + Dự báo nhu cầu tháng tới) để đưa ra một "Đơn hàng gợi ý".
### Hệ thống báo cáo và truy xuất
- Báo cáo tự động theo định kỳ qua email, tele, zalo
- Tra cứu dễ dàng, lịch sử xuất nhập