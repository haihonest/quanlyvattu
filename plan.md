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
- Tối ưu quy trình nghiệp vụ giảm thời gian nhập liệu (thay vì nhập tay có thể OCR hóa đơn và tự nhập), tự động map 
- Dự báo nhu cầu vật tư theo tuần, theo tháng, 
- Phân tích nhu cầu theo xu hướng: top vật tư tăng, vật tư giảm, vật tư bất thường
- Quản trị và kiểm soát nội bộ: audit log mọi thay đổi, phân quyền theo đúng chức năng nhiệm vụ

# Quy trình xử lý
## Quy trình nhập liệu
