# Session Log: 2026-08-03

## 1. Kết quả Công việc (Work Accomplished)
* **Task 6 (DONE & REVIEW CLEAN)**: Hoàn thành kiểm thử toàn diện và bàn giao mã nguồn. Đã fix lỗi danh hiệu theo phần trăm, bảo vệ logic ôn tập và chuẩn hóa thông số SVG Donut Chart ($502.65$).
* **Khắc phục lỗi hiển thị tiếng Việt**: Đã thay thế font `Cinzel` bằng font `Playfair Display` hỗ trợ 100% tiếng Việt có dấu cho các tiêu đề chính.
* **Đẩy dự án lên GitHub**: Thiết lập liên kết remote SSH và push toàn bộ mã nguồn lên repository: [github.com/phungducanh99/loivang](https://github.com/phungducanh99/loivang) (nhánh `main`).
* **Tính năng Đồng hồ đếm ngược 20s & Chuông báo**:
  * Tích hợp thanh đếm ngược thời gian 20 giây ở đầu mỗi câu hỏi (chuyển đỏ khi còn dưới 5 giây).
  * Phát chuông báo hết giờ tự tổng hợp qua **Web Audio API** (đảm bảo chạy offline 100% không phụ thuộc file âm thanh).
  * Hiển thị lớp phủ Glassmorphism khóa lựa chọn đáp án khi hết giờ, kèm theo nút **"Kết quả"** để người dùng chủ động nhấn hiển thị đáp án đúng và phần giải thích chi tiết.
  * Reset bộ đếm và trạng thái xem đáp án khi chuyển câu, làm lại bài, hoặc ôn tập.
* **Cập nhật tài liệu**: Cập nhật chi tiết lịch sử commit và tính năng vào [walkthrough.md](file:///Users/phungducanh/Library/CloudStorage/GoogleDrive-ducanh.gp25@gmail.com/My%20Drive/AI/Quiz-Loi-Vang/docs/superpowers/walkthrough.md).

## 2. Kiến trúc & Trạng thái Kỹ thuật (Technical State)
* **Cấu trúc**: Single Page Application (SPA) tĩnh chạy offline 100% trong một file `index.html` duy nhất.
* **State Management**: Quản lý qua mảng `userAnswers` (lưu cả đáp án đúng/sai và giá trị `-1` đại diện cho hết giờ). Trạng thái đồng bộ tự động vào `localStorage` qua key `quiz_loi_vang_p2_state`.
* **GitHub Repository**: [github.com/phungducanh99/loivang.git](https://github.com/phungducanh99/loivang.git)

## 3. Bài học Rút ra & Anti-patterns Tránh được (Lessons Learned)
* **Hỗ trợ Tiếng Việt của Font chữ**: Luôn chọn font chữ có hỗ trợ đầy đủ bộ ký tự Latin mở rộng (ví dụ: `Playfair Display`, `Lora`...) khi làm việc với văn bản tiếng Việt có dấu. Tránh dùng các font La Mã cổ đại như `Cinzel` cho văn bản tiếng Việt.
* **Sử dụng Web Audio API cho Offline SPA**: Thay vì phụ thuộc vào file âm thanh cục bộ (dễ bị lỗi sai đường dẫn tương đối khi di chuyển hoặc chạy offline), việc tự tổng hợp tần số âm thanh (oscillator) bằng Web Audio API là giải pháp tối ưu và an toàn nhất.
* **Quyền hạn SSH với nhiều tài khoản GitHub**: Lỗi phân quyền xảy ra khi máy trạm liên kết mặc định với tài khoản `ducanhgp2525` nhưng repository đích thuộc về `phungducanh99`. Khắc phục bằng cách phân quyền Collaborator trên GitHub.

## 4. Các bước Tiếp theo (Next Steps)
* Bàn giao mã nguồn hoàn chỉnh cho PDA sử dụng thực tế.
* Nhận feedback vận hành để tinh chỉnh UI/UX hoặc bổ sung các tính năng khác nếu cần.

---

# Session Log: 2026-08-04

## 1. Kết quả Công việc (Work Accomplished)
* **Tích hợp tiếng tích tắc đồng hồ (Tick-tock sound)**:
  - Thiết kế và phát âm thanh tích tắc cơ học (Triangle wave, luân phiên 1600Hz cho giây chẵn và 1200Hz cho giây lẻ) mỗi giây trong suốt 20 giây đếm ngược.
  - Âm lượng (gain) tự động tăng dần từ nhỏ (0.02) lúc 20s đến to dần (0.2) ở những giây cuối cùng để tạo tính thúc giục.
* **Tích hợp chuỗi chuông hết giờ (End bell sequence)**:
  - Thay thế tiếng chuông đơn cũ bằng chuỗi 3 hồi chuông vang dồn dập (2 tiếng 880Hz cách nhau 0.3s và 1 tiếng cao trào 1046.5Hz ngân dài 2.5s ở giây 0.6) bằng Web Audio API để thông báo hết giờ rõ ràng.
* **Sửa lỗi không nghe thấy tiếng (Bug Fix)**:
  - **Nguyên nhân**: Trình duyệt giới hạn số lượng AudioContext hoạt động đồng thời (tối đa 6-8). Việc tạo `new AudioContext()` liên tục mỗi giây trong `setInterval` đã làm sập hệ thống âm thanh của trình duyệt. Thêm vào đó, chính sách Autoplay Policy của trình duyệt chặn âm thanh tự động phát khi chưa có tương tác của người dùng.
  - **Khắc phục**: Chuyển sang sử dụng duy nhất một thực thể `audioCtx` toàn cục. Đăng ký sự kiện lắng nghe tương tác đầu tiên của người dùng (`click` / `touchstart` trên tài liệu) để tự động khởi tạo và mở khóa (`.resume()`) AudioContext.
* **Cập nhật ngân hàng câu hỏi mới**:
  - Nhận dữ liệu CSV gồm 100 câu hỏi, đáp án, và giải thích từ người dùng.
  - Viết và thực thi tập lệnh JavaScript để phân tích CSV (xử lý an toàn các dấu phẩy bên ngoài dấu ngoặc kép) và thay thế toàn bộ dữ liệu trong `quizData` của file `index.html`.
  - Xác thực thành công 100% cú pháp JavaScript và số lượng 100 câu hỏi.
* **Đồng bộ hóa Repository mới**:
  - Clone dự án `LOIVANG2` và thiết lập remote mới sang `loivang-1` tại [github.com/phungducanh99/loivang-1.git](https://github.com/phungducanh99/loivang-1.git).
  - Đẩy toàn bộ thay đổi và lịch sử commit lên nhánh `main`.

## 2. Kiến trúc & Trạng thái Kỹ thuật (Technical State)
* **Ngân hàng câu hỏi**: Đã cập nhật 100 câu hỏi Phần 1 (Từ Chương 1 đến Chương 6) từ tác phẩm "Lời Vàng Của Thầy Tôi".
* **Kiến trúc**: SPA tĩnh chạy offline 100% qua file `index.html`, sử dụng Web Audio API để phát âm thanh.
* **GitHub Repository**: [github.com/phungducanh99/loivang-1.git](https://github.com/phungducanh99/loivang-1.git)

## 3. Bài học Rút ra (Lessons Learned)
* **Xử lý CSV thủ công**: Khi phân tích chuỗi CSV tự do không tuân thủ nghiêm ngặt chuẩn định dạng (ví dụ: chứa dấu phẩy không bao bằng dấu nháy kép trong văn bản câu hỏi), việc sử dụng phân tích ngược từ cuối dòng (end-based extraction) dựa trên các cột cố định (Đáp án đúng, Giải thích, Đáp án D, C, B, A) mang lại sự chính xác tuyệt đối.

## 4. Các bước Tiếp theo (Next Steps)
* Bàn giao mã nguồn hoàn chỉnh của dự án `loivang-1` cho người dùng.

---

# Session Log: 2026-08-06

## 1. Kết quả Công việc (Work Accomplished)
* **Tích hợp hình nền tùy chỉnh**:
  - Lưu hình nền phong cách Phật giáo/mạn-đà-la do người dùng cung cấp vào dự án dưới tên [assets/background.jpg](file:///Users/phungducanh/.gemini/antigravity/scratch/loivang-1/assets/background.jpg).
  - Cấu hình body background sử dụng hình nền này kết hợp với lớp phủ gradient tối (`rgba(13, 4, 4, 0.45)` đến `rgba(28, 12, 12, 0.45)`) để đảm bảo độ tương phản cao cho chữ.
* **Hỗ trợ chế độ Sáng/Tối (Light/Dark Mode)**:
  - Thiết kế nút chuyển đổi giao diện (theme toggle) hình tròn nổi (floating button) cố định ở góc trên bên phải màn hình.
  - Xây dựng giao diện Light Mode tinh tế bằng cách làm mờ/làm sáng hình nền (phủ lớp màu trắng đục `rgba(255, 255, 255, 0.88)` lên hình nền để giữ hoa văn mạn-đà-la chìm) và chuyển đổi các tông màu tối sang tông màu kem sáng, chữ tối.
  - Lưu trạng thái lựa chọn giao diện vào `localStorage` để tự động khôi phục khi tải lại trang, đi kèm hiệu ứng chuyển cảnh mượt mà (`transition: 0.4s ease`).
* **Kiểm thử & Đồng bộ**:
  - Đảm bảo 100% cú pháp JavaScript không lỗi.
  - Commit và push code lên GitHub.

## 2. Kiến trúc & Trạng thái Kỹ thuật (Technical State)
* **Theme System**: Quản lý bằng lớp `light-theme` trên `<body>` và ghi đè các biến CSS để thay đổi diện mạo tức thì.
* **Assets**: [assets/background.jpg](file:///Users/phungducanh/.gemini/antigravity/scratch/loivang-1/assets/background.jpg).

## 3. Bài học Rút ra (Lessons Learned)
* **Khai thác hình nền tối cho Light Mode**: Để giữ nguyên các hoa văn tinh xảo của hình nền tối trong Light Mode mà không cần chuẩn bị thêm hình nền thứ hai, phương pháp xếp chồng một dải màu trắng đục bán trong suốt (ví dụ: `linear-gradient(rgba(255,255,255,0.88), rgba(255,255,255,0.88))`) lên trước ảnh nền trong thuộc tính `background` mang lại kết quả hiển thị nền sáng vân mờ cực kỳ sang trọng và tiết kiệm tài nguyên.

---

# Session Log: 2026-08-07

## 1. Kết quả Công việc (Work Accomplished)
* **Bổ sung 25 câu hỏi tích hợp sâu**:
  - Ghép thêm 25 câu hỏi trắc nghiệm phức hợp nâng cao (số thứ tự từ 101 đến 125) vào cuối mảng dữ liệu `quizData`.
  - Tự động hóa việc phân tích đáp án (A/B/C/D thành 0/1/2/3) và gán mức độ `"hard"`.
* **Tối ưu hóa bố cục hiển thị trang đơn (Fit Viewport)**:
  - Thiết kế riêng lớp CSS `.long-text` cho thẻ chứa câu hỏi từ câu 101 trở đi.
  - Giảm kích thước font chữ tiêu đề (`1.3rem`), thu nhỏ padding (`24px 30px`), khoảng cách và padding của các nút lựa chọn để đảm bảo nội dung câu hỏi rất dài vẫn hiển thị gọn gàng trên cùng 1 trang màn hình mà không cần cuộn (chống tràn viewport).
  - Hỗ trợ responsive cho màn hình di động nhỏ để hiển thị tối ưu.
* **Mở rộng thời gian suy nghĩ cho các câu hỏi nâng cao**:
  - Điều chỉnh thời gian đếm ngược lên **40 giây** (thay vì 20 giây mặc định) cho tất cả các câu hỏi từ 101 trở đi.
  - Tự động thay đổi phần trăm thanh thời gian chạy và nhãn hiển thị tương thích theo mức giới hạn động.

## 2. Kiến trúc & Trạng thái Kỹ thuật (Technical State)
* **Question Database**: Tổng cộng 125 câu hỏi trắc nghiệm Phật Pháp hoàn chỉnh.
* **Timer Logic**: Hệ thống đếm ngược động thông qua hàm `getQuestionTimeLimit()`.


