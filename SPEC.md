# SPECIFICATION & RULES: ADOBE ILLUSTRATOR PANEL (HTML/CSS)

## 1. Bản chất dự án & Môi trường thực thi
- **Target environment**: Panel plugin nhúng bên trong Adobe Illustrator (sử dụng CEP/UXP WebView).
- **Format UI**: Giao diện dạng CỘT DỌC (Sidebar/Dock panel), tương tự UI mobile app. Tuyệt đối KHÔNG dàn layout ngang kiểu desktop web thông thường.
- **Tech stack khuyến nghị**: Vanilla HTML5, Vanilla CSS (hoặc SCSS compile sạch), không dùng thư viện CSS quá nặng.

## 2. Kích thước & Layout Foundation (Fluid & Responsive)
- **Container constraint & Resizing**: 
  - Giao diện có thể co giãn tự do theo thao tác kéo rộng/hẹp panel của người dùng trong Illustrator.
  - Tuyệt đối KHÔNG fix cứng `width: 380px` hay dùng `100vw`, `100vh`.
  - Thiết lập chuẩn: `width: 100%; min-width: 300px; min-height: 100vh; box-sizing: border-box; display: flex; flex-direction: column;`.
- **Cơ chế Fluid Flexbox**:
  - Các nút bấm hành động, ô input, sub-tabs tự dãn đều theo chiều ngang (`width: 100%` hoặc `flex: 1`).
  - Trong bảng dữ liệu (Table/List): Cột "Tên mẫu" (input text) dùng `flex: 1; min-width: 0;` để tự chiếm toàn bộ khoảng trống dôi ra khi kéo rộng panel. Các cột Hình, Rộng, Cao, Xóa dùng kích thước cố định kèm `flex-shrink: 0;`.
- **Scrolling**:
  - `overflow-x: hidden;` cho toàn bộ app để chặn triệt để thanh trượt ngang.
  - Container danh sách hoặc toàn trang dùng `overflow-y: auto;` để người dùng cuộn nội dung mượt mà khi panel bị co ngắn lại theo chiều dọc.
- **Bố cục nội bộ**: Ưu tiên 100% `Flexbox` thay vì CSS Grid phức tạp để đảm bảo tương thích mượt mà trên bộ render của Adobe UXP/CEP.

## 3. Quản lý Assets & Tài nguyên
- **100% Offline/Local**:
  - Tuyệt đối KHÔNG dùng link CDN online (Google Fonts, FontAwesome CDN, Unpkg,...). Plugin chạy offline trong Illustrator sẽ bị mất toàn bộ style/icon.
  - Toàn bộ Icon (cờ, dấu tick, thùng rác, folder, icon setting, dropdown arrow) phải dùng SVG nội bộ: để trong thư mục `./assets/` hoặc dùng SVG inline trực tiếp.
  - Font chữ dùng font hệ thống sạch (system-ui, sans-serif) hoặc nhúng font file cục bộ (woff/woff2/ttf).

## 4. Trải nghiệm App Native
- **Chặn bôi xanh chữ**:
  ```css
  body {
    user-select: none;
    -webkit-user-select: none;
  }
  /* Cho phép chọn/gõ text ở các trường nhập liệu */
  input, select, textarea {
    user-select: text;
    -webkit-user-select: text;
  }