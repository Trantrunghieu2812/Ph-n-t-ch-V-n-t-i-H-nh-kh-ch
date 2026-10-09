Dashboard Phân tích Vận tải Hành khách (Power BI)
Dự án Data Analyst – Phân tích booking, tuyến đường, khuyến mãi và hành vi khách hàng trên nền tảng đặt vé xe khách
· Công cụ: Power BI, DAX, Power Query · Dữ liệu: 2 năm (2024–2025)
1. Tổng quan dự án
Bối cảnh
Nền tảng đặt vé xe khách bán vé của nhiều nhà xe qua nhiều kênh (Web, App, đại lý…) và thu hoa hồng trên mỗi vé hoàn tất. Doanh thu của nền tảng chịu ảnh hưởng bởi ba yếu tố: mùa vụ, tỷ lệ hủy vé và chi phí khuyến mãi. Ban điều hành cần một báo cáo trả lời nhanh: tiền đang đến từ đâu, thất thoát ở đâu và nên hành động gì trước.
Mục tiêu
• Theo dõi sức khỏe kinh doanh qua các chỉ số chính: GMV, số booking, hoa hồng, tỷ lệ hủy.
• Tìm tuyến, kênh và giai đoạn đang làm tăng hoặc giảm hiệu quả.
• Đánh giá chương trình khuyến mãi có thực sự tạo ra giá trị hay không.
• Đưa ra đề xuất hành động có thứ tự ưu tiên.
Câu hỏi kinh doanh
1. GMV và hoa hồng biến động thế nào theo tháng, theo năm?
2. Nhu cầu tập trung vào giai đoạn nào, và vận hành có đáp ứng kịp không?
3. Tuyến và kênh nào có tỷ lệ hủy cao, gây thất thoát bao nhiêu?
4. Mỗi đồng giảm giá thu về được bao nhiêu hoa hồng?
2. Dữ liệu và mô hình
Bộ dữ liệu gồm 8 bảng theo mô hình sao (star schema):
fact_bookings- Giá vé: giá vé, số ghế, trạng thái, voucher, ngày đặt, ngày khởi hành
fact_trips - Chuyến xe:sức chứa và mức lấp đầy
dim_date - Lịch (ngày, tháng, năm, thứ)
dim_route - Tuyến đường
dim_operator - Nhà xe
dim_channel - Kênh bán
dim_customer - Khách hàng
dim_voucher - Mã khuyến mãi và ngân sách
Lưu ý về mô hình
• fact_bookings nối dim_date bằng hai quan hệ: booking_date (đang dùng) và departure_date (tạm tắt, kích hoạt bằng USERELATIONSHIP).
• dim_date được đánh dấu là Date table để các hàm so sánh cùng kỳ hoạt động đúng.
• Mọi measure nằm trong bảng _Measures và được chia theo Display folder.
• Ngày đặt vé dùng cho xu hướng và hủy vé. Ngày khởi hành dùng cho mùa vụ và vận hành.
Các chỉ số chính (DAX)
• GMV = tổng (giá vé × số ghế) của vé có trạng thái Completed.
• GMV YoY % so sánh cùng kỳ năm trước bằng SAMEPERIODLASTYEAR.
• HH trên chi phí = hoa hồng từ booking có voucher ÷ chi phí giảm giá. Dưới 1,0 nghĩa là nền tảng trả nhiều hơn thu.
• Tỷ lệ hủy và khoảng chênh hủy vé theo tuyến và kênh.
3. Các trang dashboard
Trang 1 – Tổng quan điều hành
Câu hỏi: Kinh doanh đang tăng hay giảm, và các điểm nghẽn chính là gì?
• Thẻ KPI có sparkline và so sánh cùng kỳ: GMV, số booking, hoa hồng, tỷ lệ hủy.
• Biểu đồ GMV theo tháng, cơ cấu theo kênh bán, so sánh 2 năm.
• Bộ lọc năm đồng bộ qua các trang.
Trang 2 – Mùa vụ và nhu cầu
Câu hỏi: Nhu cầu tập trung vào thời điểm nào?
• Booking theo ngày khởi hành và theo thứ trong tuần.
• Bản đồ nhiệt thứ × tháng để thấy giai đoạn cao điểm.
• Mức lấp đầy ở cao điểm so với ngày thường.
Trang 3 – Hiệu quả tuyến đường
Câu hỏi: Tuyến nào đáng đầu tư, tuyến nào cần xem lại?
• Ma trận mức lấp đầy × hoa hồng mỗi chuyến.
• Bảng chi tiết từng tuyến để so sánh nhanh.
Trang 4 – Hiệu quả khuyến mãi
Câu hỏi: Mỗi đồng giảm giá thu về được bao nhiêu hoa hồng?
• Hoa hồng trên mỗi đồng giảm giá của từng mã, có đường hòa vốn 1,0.
• Chi phí giảm giá so với hoa hồng thu về, theo mã và theo tháng.
• Tỷ lệ voucher được dùng bởi đơn đầu tiên, để phát hiện rò rỉ.
• Bảng chi tiết từng mã, tô màu theo hiệu quả.
4. Insight chính
Khuyến mãi
• Khoảng 27,5% booking (67.456 lượt) có dùng voucher.
• Chi phí giảm giá là 2,38 tỷ, còn hoa hồng thu về từ các booking này là 2,06 tỷ, tương đương 0,86 đồng hoa hồng cho mỗi đồng giảm giá. Nền tảng đang chi nhiều hơn thu.
• VEX5 và FLASH30K hiệu quả (trên 1,0). WELCOME50K, HE10 và TET15 đang lỗ (dưới 1,0).
• WELCOME50K bị rò rỉ: khoảng 46% lượt dùng đến từ khách không phải đơn đầu, trong khi mã này sinh ra để thu hút khách mới.
• Chi phí giảm giá vượt hoa hồng rõ rệt ở đỉnh Tết (tháng 1/2025).
• Voucher chưa cho thấy tác dụng rõ rệt: giá trị đơn trung bình 401K (có voucher) so với 383K (không voucher), tỷ lệ hủy 8,9% so với 9,0%.
Hủy vé
• Khoảng chênh hủy vé tập trung ở kênh Web (khoảng 0,97 tỷ) và tuyến Sa Pa (khoảng 0,51 tỷ).
Mùa vụ
• Nhu cầu tăng mạnh vào dịp Tết và cao điểm, nên cần so sánh với mức lấp đầy để biết vận hành có kịp đáp ứng không.
Ghi chú: các con số có thể thay đổi nhẹ tùy cách tính và bộ lọc. Hãy đối chiếu lại với dashboard trước khi trình bày.
5. Kỹ năng thể hiện qua dự án
• Mô hình hóa dữ liệu dạng sao, xử lý hai quan hệ ngày với USERELATIONSHIP.
• Viết DAX: so sánh cùng kỳ, định dạng có điều kiện, measure theo ngữ cảnh.
• Thiết kế dashboard nhiều trang: thẻ KPI, sparkline, bản đồ nhiệt, ma trận, bảng tô màu.
• Tư duy phân tích: từ câu hỏi kinh doanh, đến insight, đến đề xuất có ưu tiên.
• Biết giới hạn của dữ liệu: dữ liệu mô phỏng nên kết luận về voucher và giữ chân khách hàng cần kiểm chứng thêm.
• Dữ liệu có tính mô phỏng, nên mức độ tác động của voucher cần thử nghiệm thực tế.
• Chưa có dữ liệu chi phí vận hành nên chưa tính được lợi nhuận thực.
• Hướng mở rộng: phân khúc khách hàng (RFM), dự báo nhu cầu theo mùa, cảnh báo tự động khi tỷ lệ hủy tăng.
