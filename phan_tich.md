

**Phương án lựa chọn:** Phương án B

**Phân tích phương án chọn:**
Phương án B cung cấp một mô tả ngữ nghĩa (semantic description) tối ưu nhất, phù hợp với cơ chế hoạt động của Function Calling trong các mô hình LLM.
- **Tính rõ ràng và đầy đủ về ngữ cảnh (Context):** LLM không cần biết code bên dưới chạy như thế nào, nó chỉ cần biết "hàm này làm gì", "khi nào nên gọi hàm này", và "cần truyền vào những thông tin gì". Phương án B mô tả rất rõ mục đích của từng hàm (kiểm tra phòng trống, tính tổng chi phí).
- **Định dạng và giới hạn tham số cụ thể:** Bằng cách chỉ rõ các tham số cần thiết (`checkInDate`, `checkOutDate`, `roomType`) và cả định dạng dữ liệu (ví dụ: `yyyy-MM-dd`), LLM sẽ hiểu rõ cách để trích xuất hoặc định dạng dữ liệu từ câu thoại của người dùng trước khi gọi hàm. Điều này giúp ngăn chặn các lỗi (như lỗi parse ngày tháng) ở phía backend do AI truyền sai format dữ liệu.
- **Ràng buộc về logic và luồng thực thi:** Trong hàm `calculateTotalPrice`, việc ghi chú "chỉ được gọi sau khi đã xác định được loại phòng... và tổng số ngày..." là một dạng System Instruction nhúng trong Tool Description. Nó định hướng cho LLM về quy trình/thứ tự công việc (workflow). Nhờ đó, LLM sẽ không "cầm đèn chạy trước ô tô", tránh việc gọi hàm tính tiền nếu chưa có đủ thông tin.

**Phân tích các phương án loại trừ:**
- **Phương án A (Mô tả tối giản):** 
  - *Nhược điểm và Rủi ro:* Quá sơ sài và thiếu ngữ cảnh. LLM có thể hiểu hàm dùng để làm gì, nhưng nó sẽ không biết chính xác cần truyền tham số gì hay định dạng thời gian ra sao (nó có thể truyền `20-10-2023` thay vì `2023-10-20` gây lỗi hệ thống). Ngoài ra, do không quy định rõ điều kiện gọi hàm, AI có thể bị ảo giác (hallucination) gọi hàm `calculateTotalPrice` với các số liệu tự bịa ra khi người dùng chưa cung cấp đủ thông tin ngày ở.

- **Phương án C (Mô tả kỹ thuật nội bộ):**
  - *Nhược điểm và Rủi ro:* Bị "ô nhiễm" thông tin (Information Noise) và rò rỉ kiến trúc. Mô hình LLM đóng vai trò là cầu nối giao tiếp ngôn ngữ tự nhiên, không phải là trình biên dịch. Các chi tiết cài đặt bên trong như `class BookingService`, `MySQL DB`, hay `JPA` là hoàn toàn thừa thãi với LLM. Việc đưa vào không chỉ làm tốn chi phí (tiêu tốn nhiều token vô ích) mà còn khiến AI bị phân tâm. Nghiêm trọng hơn, mô tả này có thể khiến AI tiết lộ thông tin nhạy cảm về kiến trúc hạ tầng và cơ sở dữ liệu của hệ thống cho người dùng cuối (lỗ hổng bảo mật).
