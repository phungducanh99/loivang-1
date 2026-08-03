# Thiết kế Ứng dụng Trắc nghiệm Lời Vàng Của Thầy Tôi - Phần Hai

Tài liệu thiết kế chi tiết cho ứng dụng trắc nghiệm tương tác 100 câu hỏi thuộc Phần Hai: "Những Pháp Tu Dự Bị Phi Thường" của tác phẩm "Lời Vàng Của Thầy Tôi" do Đại sư Patrul Rinpoche biên soạn.

---

## 1. Yêu cầu Hệ thống & Kiến trúc
* **Kiến trúc**: Ứng dụng đơn trang (Single Page Application - SPA) chạy tĩnh hoàn toàn phía Client.
* **Công nghệ**: HTML5, CSS3 hiện đại (nhúng trong file HTML), Javascript thuần (ES6+).
* **Định dạng file**: Một file duy nhất `index.html` chứa toàn bộ cấu trúc, phong cách thiết kế và logic xử lý để người dùng có thể dễ dàng chạy offline bằng cách click đúp.

---

## 2. Thiết kế Mỹ thuật & Giao diện (UI/UX)
Giao diện được thiết kế theo phong cách **Dark Mandala & Glassmorphism**, kết hợp màu sắc trang trọng của Phật giáo Tây Tạng và các hiệu ứng mượt mà của web hiện đại.

### 2.1. Bảng màu (Color Palette)
* **Nền tổng thể (App Background)**: Gradient chuyển sắc mượt mà góc chéo từ đỏ sẫm Tây Tạng (`#0d0404`) sang nâu đất ấm (`#1c0c0c`).
* **Hộp câu hỏi (Question Card)**: Lớp kính mờ (`rgba(35, 15, 15, 0.7)`) với thuộc tính `backdrop-filter: blur(12px)`. Viền mảnh vàng kim nhạt (`rgba(212, 175, 55, 0.25)`).
* **Màu chữ chính (Primary Text)**: Trắng ngà ấm (`#f7f1e5`) để đọc lâu không mỏi mắt.
* **Màu chữ phụ (Secondary Text)**: Màu cát nhạt (`#c7bba8`).
* **Màu nhấn (Accent Color)**: Vàng kim Tây Tạng (`#d4af37`).
* **Màu trạng thái đáp án**:
  * **Đúng**: Xanh ngọc lục bảo ấm (`#2e7d32`) kết hợp hiệu ứng đổ bóng phát sáng nhẹ (`box-shadow: 0 0 8px rgba(46, 125, 50, 0.5)`).
  * **Sai**: Đỏ ruby sẫm (`#a61c1c`) kết hợp hiệu ứng đổ bóng đỏ nhạt.

### 2.2. Typography
* Tiêu đề chính sử dụng Google Font `'Playfair Display'` sang trọng và trang nghiêm.
* Nội dung câu hỏi và các tùy chọn sử dụng Google Font `'Plus Jakarta Sans'` giúp tăng tối đa độ sắc nét và khả năng đọc trên màn hình di động/máy tính.

### 2.3. Các thành phần giao diện chính
1. **Header**: Tiêu đề chính căn giữa, chữ vàng kim, đi kèm đường kẻ ngang gradient mảnh và subtitle chỉ rõ nội dung Phần Hai.
2. **Thanh tiến trình (Progress Bar)**: Thanh trượt chạy mượt mà phía trên card câu hỏi, tự động cập nhật độ rộng dựa trên tỷ lệ câu hiện tại/tổng số câu (`(currentQuestion + 1) / totalQuestions * 100%`).
3. **Card Câu Hỏi**:
   * Tiêu đề câu hỏi hiển thị to, rõ kèm theo Badge chỉ mức độ khó (Dễ: Xanh lá `#10b981`, Trung bình: Vàng hổ phách `#f59e0b`, Khó: Đỏ san hô `#ef4444`).
   * Các đáp án hiển thị dạng nút bấm dạng thẻ (Card-like button), bo góc 8px, căn trái.
   * Hiệu ứng hover: Phóng to nhẹ (`transform: translateY(-2px)`), đổi viền sang vàng kim sáng.
4. **Hộp thoại Giải thích (Explanation Section)**:
   * Ẩn theo mặc định. Khi người dùng chọn đáp án, hộp giải thích sẽ trượt mở rộng mượt mà (`max-height` transition) hiển thị nội dung phân tích chi tiết giáo lý.
5. **Bản đồ câu hỏi nhanh (Quiz Navigation Map)**:
   * Nút bấm biểu tượng lưới ở góc phải cho phép mở ra một Modal chứa 100 ô lưới đại diện cho 100 câu hỏi.
   * Trạng thái các ô lưới:
     * Màu xám tối: Chưa trả lời.
     * Màu xanh lá: Trả lời đúng.
     * Màu đỏ: Trả lời sai.
   * Người dùng có thể click vào bất kỳ ô nào để nhảy nhanh tới câu hỏi tương ứng.
6. **Màn hình Kết quả (Result Screen)**:
   * Hiển thị điểm số dạng vòng tròn tiến trình động (SVG Animated Ring) tự quay mượt mà.
   * Phân loại danh hiệu tu học dựa trên điểm số:
     * **>= 90 điểm**: Bậc Thượng Căn (Trí tuệ thấu suốt sâu sắc).
     * **70 - 89 điểm**: Bậc Trung Căn (Hiểu biết sâu rộng giáo lý).
     * **50 - 69 điểm**: Bậc Hạ Căn (Nền tảng khá vững chắc).
     * **< 50 điểm**: Người Mới Bắt Đầu (Cần tiếp tục đọc và quán chiếu sâu hơn).
   * Cung cấp nút "Xem lại đáp án" (Review Mode) và nút "Làm lại từ đầu" (Restart).

---

## 3. Logic Xử lý & Các Tính năng Nâng cấp
* **Lưu Tiến trình Tự động (Auto-resume)**:
  * Mỗi khi người dùng trả lời một câu hỏi, ứng dụng sẽ lưu trạng thái (`currentQuestion`, `score`, mảng `userAnswers`) vào `localStorage` của trình duyệt.
  * Khi người dùng tải lại trang, nếu phát hiện có tiến trình cũ chưa hoàn thành, ứng dụng sẽ hiển thị một Modal trang nhã hỏi: "Bạn có muốn tiếp tục làm bài trắc nghiệm từ câu hỏi thứ X không?". Nếu đồng ý, khôi phục trạng thái và tiếp tục. Nếu từ chối, xóa bộ nhớ tạm và bắt đầu lại từ đầu.
* **Xem lại câu hỏi (Review Mode)**:
  * Sau khi kết thúc bài trắc nghiệm, người dùng có thể kích hoạt chế độ xem lại để duyệt qua toàn bộ 100 câu hỏi, xem đáp án mình đã chọn (đúng/sai) cùng phần giải thích tương ứng của từng câu mà không bị khóa nút bấm.

---

## 4. Kế hoạch Xác minh (Verification Plan)
* **Kiểm tra hiển thị (UI Verification)**:
  * Kiểm tra giao diện hiển thị chính xác trên Safari, Chrome (máy tính) và Safari Mobile (iOS).
  * Đảm bảo hiệu ứng Glassmorphism hoạt động tốt và chữ có độ tương phản cao, dễ đọc.
* **Kiểm tra Logic**:
  * Kiểm tra tính năng lưu/khôi phục tiến trình thông qua `localStorage`.
  * Kiểm tra tính năng chuyển câu hỏi nhanh thông qua Bản đồ câu hỏi (Quiz Map).
  * Xác thực việc tính điểm số chính xác và vẽ vòng tròn SVG kết quả tương ứng.
