# Walkthrough: Ứng dụng Trắc nghiệm Lời Vàng Của Thầy Tôi

Tài liệu tổng kết kết quả hoàn thành dự án Trắc nghiệm tương tác 100 câu hỏi Phần Hai "Những Pháp Tu Dự Bị Phi Thường".

---

## 1. Kết quả Triển khai
Ứng dụng đã được xây dựng thành công dưới dạng **Single Page Application (SPA)** tĩnh, tích hợp toàn bộ HTML, CSS và JavaScript trong một file duy nhất:
* 📂 **Đường dẫn file**: `/Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My Drive/AI/Quiz-Loi-Vang/index.html`

Anh chỉ cần **click đúp vào file `index.html`** để mở và làm bài trắc nghiệm ngay trên bất kỳ trình duyệt nào (Safari, Chrome, Firefox...) mà không cần kết nối mạng.

---

## 2. Các Tính năng Nổi bật (Đã Triển Khai)

### 2.1. Giao diện Dark Mandala & Glassmorphism
* Phối màu tối trầm ấm đậm chất Phật giáo Tây Tạng kết hợp viền vàng kim tinh tế.
* Hiệu ứng kính mờ Glassmorphism trên thẻ câu hỏi giúp tăng tính hiện đại và sang trọng.
* Typography sắc nét sử dụng font chữ nghệ thuật `Playfair Display` cho tiêu đề (hỗ trợ hoàn hảo tiếng Việt có dấu) và font sans-serif dễ đọc `Plus Jakarta Sans` cho nội dung câu hỏi.

### 2.2. Tự động Lưu & Khôi phục Tiến trình (Auto-save / Resume)
* Hệ thống tự động lưu trạng thái làm bài (câu hỏi hiện tại, đáp án đã chọn, điểm số) vào `localStorage` của trình duyệt sau mỗi thao tác.
* Khi tải lại trang, nếu có tiến trình dở dang, một Modal thông báo sẽ xuất hiện hỏi anh có muốn tiếp tục làm tiếp hay không.

### 2.3. Bản đồ Câu hỏi Nhanh (Quiz Map)
* Nút bấm **"🗺️ Bản đồ câu hỏi"** hiển thị lưới 100 ô đại diện cho 100 câu hỏi trắc nghiệm.
* Trạng thái màu sắc của lưới:
  * **Xám tối**: Câu hỏi chưa trả lời.
  * **Xanh ngọc**: Câu hỏi đã trả lời đúng.
  * **Đỏ ruby**: Câu hỏi đã trả lời sai.
  * **Viền vàng**: Câu hỏi hiện tại đang làm.
* Anh có thể click vào bất kỳ ô nào để nhảy nhanh đến câu hỏi đó.

### 2.4. Biểu đồ Điểm số SVG & Phân loại Danh hiệu
* Màn hình kết quả hiển thị biểu đồ Donut Chart SVG tự động quay vẽ nét tiến trình sinh động.
* Tự động tính toán tỷ lệ đúng và phong danh hiệu tu học tương ứng (Thượng Căn, Trung Căn, Hạ Căn, Người Mới Bắt Đầu) kèm theo lời nhắn trang trọng.

### 2.5. Chế độ Ôn tập (Review Mode)
* Cho phép anh duyệt lại toàn bộ 100 câu hỏi để xem đáp án đúng/sai và phần giải thích chi tiết tôn giáo mà không làm ảnh hưởng đến điểm số hay trạng thái làm bài trước đó.

### 2.6. Đồng hồ đếm ngược 20 giây & Chuông báo (Timer & Audio Bell)
* Mỗi câu hỏi được trang bị một **thanh đếm ngược thời gian 20 giây** tự động chạy. Thanh sẽ chuyển đỏ cảnh báo khi còn dưới 5 giây.
* **Tự tổng hợp âm thanh (Web Audio API)**: Khi hết 20 giây, một tiếng chuông ngân vang thanh tịnh sẽ vang lên mà không cần bất kỳ file audio phụ thuộc bên ngoài nào (đảm bảo chạy offline 100%).
* **Lớp phủ hết giờ (Timeout Overlay) & Nút hiển thị**: Một lớp kính mờ màu đỏ tối xuất hiện che khu vực lựa chọn đáp án để khóa tương tác. Trên lớp phủ tích hợp nút **"Kết quả"** – đáp án đúng và phần Giải thích giáo lý chỉ hiển thị khi người dùng chủ động click nút này, giúp duy trì tính bất ngờ và tăng hiệu quả quán chiếu học tập.
* **Auto-save**: Đồng bộ trạng thái hết giờ (đánh dấu chưa trả lời đúng) vào LocalStorage. Timer sẽ tự động dừng khi người dùng đã trả lời hoặc khi ở chế độ Ôn tập (Review Mode).

---

## 3. Nhật ký Commit (Git History)
* `11ff6bf` - feat: show 'Kết quả' button on timeout overlay to reveal answer manually
* `a99567b` - feat: add 20s countdown timer, bell sound, and timeout overlay
* `cabe10e` - fix: replace Cinzel with Playfair Display to support Vietnamese diacritics
* `249972e` - fix(task-6): perform full testing and apply final quality refinements
* `ab37785` - feat: implement result screen with donut chart and review mode
* `dacfadd` - feat(task-4): add auto-save, quiz map modal, and slide-down explanation
* `c88677e` - style: add Dark Mandala & Glassmorphism CSS
* `64d3b2d` - fix: disable options after selection and prevent double scoring
* `cd50632` - feat: nhúng quizData 100 câu và thiết lập các biến trạng thái JS
* `8691444` - feat: khởi tạo cấu trúc index.html cơ bản
