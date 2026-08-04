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
