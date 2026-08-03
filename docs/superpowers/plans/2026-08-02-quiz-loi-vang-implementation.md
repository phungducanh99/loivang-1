# Quiz Loi Vang Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Xây dựng một file HTML trắc nghiệm 100 câu hỏi chất lượng cao, giao diện "Dark Mandala & Glassmorphism" đẹp mắt, có lưu tiến trình tự động và điều hướng nhanh bằng bản đồ câu hỏi.

**Architecture:** Ứng dụng Single Page Application chạy tĩnh hoàn toàn trên trình duyệt Client. Tất cả tài nguyên (HTML, CSS, JS, Fonts) được tích hợp trong file `index.html` để chạy offline.

**Tech Stack:** HTML5, CSS3, Javascript thuần (ES6+).

## Global Constraints
- File đầu ra duy nhất: `/Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My Drive/AI/Quiz-Loi-Vang/index.html`
- Giao diện phải mang phong cách sang trọng, đậm nét Phật giáo Tây Tạng, bóng bẩy và responsive.
- Không sử dụng thư viện CSS bên ngoài (như Tailwind), dùng CSS thuần có sử dụng các biến CSS (`:root`) để dễ bảo trì.
- Hỗ trợ lưu tự động trạng thái làm bài vào `localStorage`.

---

### Task 1: Khởi tạo cấu trúc dự án và file index.html cơ bản

**Files:**
- Create: `/Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My Drive/AI/Quiz-Loi-Vang/index.html`

- [ ] **Step 1: Viết mã HTML cơ bản của index.html**
  Tạo cấu trúc tài liệu cơ bản, tích hợp Google Fonts (Cinzel và Plus Jakarta Sans) cùng các thẻ meta SEO cơ bản.
  
  ```html
  <!DOCTYPE html>
  <html lang="vi">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Quiz Tương Tác 100 Câu: Lời Vàng Của Thầy Tôi - Phần Hai</title>
      <!-- Google Fonts -->
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600;800&family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
      <style>
          /* CSS sẽ được thêm ở Task 3 */
      </style>
  </head>
  <body>
      <div class="app-container">
          <!-- Cấu trúc HTML chính -->
      </div>
      <script>
          // Script sẽ được thêm ở Task 2
      </script>
  </body>
  </html>
  ```

- [ ] **Step 2: Commit và kiểm định ban đầu**
  Mở file trên trình duyệt để kiểm tra cấu trúc HTML cơ bản tải thành công.

---

### Task 2: Thiết lập cơ sở dữ liệu Quiz và các biến trạng thái Javascript

**Files:**
- Modify: `/Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My Drive/AI/Quiz-Loi-Vang/index.html`

- [ ] **Step 1: Tích hợp mảng dữ liệu 100 câu hỏi**
  Chuyển toàn bộ dữ liệu 100 câu hỏi từ mã nguồn của người dùng vào khối `<script>`.
- [ ] **Step 2: Định nghĩa các biến trạng thái toàn cục**
  Định nghĩa các biến:
  ```javascript
  let currentQuestion = 0;
  let score = 0;
  let userAnswers = new Array(quizData.length).fill(null);
  ```
- [ ] **Step 3: Thiết lập hàm khởi chạy**
  Xây dựng khung các hàm hiển thị: `renderQuestion()`, `selectOption(index)`, `nextQuestion()`, `prevQuestion()`.

---

### Task 3: Phát triển phong cách thiết kế CSS (Dark Mandala & Glassmorphism)

**Files:**
- Modify: `/Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My Drive/AI/Quiz-Loi-Vang/index.html`

- [ ] **Step 1: Viết các biến CSS và Reset**
  Xây dựng các CSS variables cho màu sắc chủ đạo, font chữ và căn chỉnh cơ bản của body.
  
  ```css
  :root {
      --bg-gradient: linear-gradient(135deg, #0d0404 0%, #1c0c0c 100%);
      --card-bg: rgba(35, 15, 15, 0.7);
      --border-color: rgba(212, 175, 55, 0.25);
      --gold-accent: #d4af37;
      --text-main: #f7f1e5;
      --text-sub: #c7bba8;
      --correct: #2e7d32;
      --incorrect: #a61c1c;
      --font-title: 'Cinzel', serif;
      --font-body: 'Plus Jakarta Sans', sans-serif;
  }
  ```

- [ ] **Step 2: Thiết kế Glassmorphism Card và Buttons**
  Tạo giao diện mờ ảo cho hộp câu hỏi (`backdrop-filter: blur(12px)`) và thiết kế hiệu ứng hover phóng to nhẹ cho nút lựa chọn.
- [ ] **Step 3: Thiết kế Layout Responsive**
  Đảm bảo container chính co giãn tốt trên màn hình hẹp (mobile) bằng Flexbox/CSS Grid và Media Queries.

---

### Task 4: Viết các tính năng tương tác nâng cao (Auto-Save, Quiz Map)

**Files:**
- Modify: `/Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My Drive/AI/Quiz-Loi-Vang/index.html`

- [ ] **Step 1: Triển khai tính năng Auto-Save & Resume**
  Viết code lưu trạng thái vào `localStorage` mỗi khi trả lời câu hỏi và hiển thị Popup thông báo khi load lại trang.
- [ ] **Step 2: Xây dựng Bản đồ câu hỏi (Quiz Map)**
  Tạo cấu trúc lưới 100 nút bấm tương ứng 100 câu hỏi, hiển thị trạng thái màu và xử lý sự kiện nhảy nhanh đến câu hỏi bất kỳ.
- [ ] **Step 3: Xử lý hiển thị giải thích (Explanation Slide-Down)**
  Sử dụng CSS transition và JS để hiển thị mượt mà phần giải thích tôn giáo sau khi lựa chọn đáp án.

---

### Task 5: Thiết kế màn hình Kết quả với biểu đồ SVG động

**Files:**
- Modify: `/Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My Drive/AI/Quiz-Loi-Vang/index.html`

- [ ] **Step 1: Thiết kế vòng tròn điểm số SVG**
  Tạo một biểu đồ SVG dạng vòng tròn (donut chart) có hiệu ứng chuyển động nét vẽ (`stroke-dashoffset` transition) hiển thị phần trăm điểm số đạt được.
- [ ] **Step 2: Xử lý danh hiệu phân loại**
  Tính toán điểm số để phân loại người chơi thành các Bậc căn cơ theo giáo lý và hiển thị phản hồi thích hợp.
- [ ] **Step 3: Triển khai chế độ Ôn tập (Review Mode)**
  Viết logic cho phép người dùng click "Xem lại đáp án" để chuyển trang sang trạng thái xem lại (Review Mode) toàn bộ câu hỏi mà không làm mất điểm số.

---

### Task 6: Kiểm thử toàn diện và bàn giao

**Files:**
- Modify: `/Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My Drive/AI/Quiz-Loi-Vang/index.html`

- [ ] **Step 1: Chạy thử toàn bộ 100 câu hỏi**
  Xác minh các đáp án hoạt động chính xác, màu sắc Đúng/Sai sáng lên đúng vị trí.
- [ ] **Step 2: Kiểm tra responsive**
  Kiểm tra hiển thị của ứng dụng trên cả màn hình di động nhỏ và máy tính bảng.
