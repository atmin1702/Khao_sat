# Tóm tắt Báo cáo: Quản lý ứng dụng học từ vựng

Tóm tắt các thông tin quan trọng nhất từ file báo cáo phục vụ cho việc ôn tập và bảo vệ bài tập lớn.

## 1. Thông tin chung về đề tài
*   **Tên đề tài:** Quản lý ứng dụng học từ vựng (trên nền tảng Android).
*   **Môn học:** Phát triển ứng dụng cho các thiết bị di động (MAD).
*   **Sinh viên thực hiện:** Trần Trí Dương - B24DTCN098.
*   **Ngôn ngữ lập trình:** Java.
*   **Cơ sở dữ liệu:** SQLite (Lưu trữ cục bộ - Offline).
*   **Kiến trúc phần mềm:** Local Database Architecture kết hợp **mô hình MVC** (Model - View - Controller).
*   **Mục tiêu cốt lõi:** Xây dựng một cuốn "sổ tay từ vựng điện tử" có khả năng phân loại theo chủ đề, lưu trữ đầy đủ thông tin (từ, nghĩa, phát âm, ví dụ) và hoạt động 100% offline.

## 2. Các chức năng chính (Yêu cầu hệ thống)
*   **Quản lý Chủ đề (Topic):** Thêm mới (nhập mã, tên), Cập nhật (đổi tên), Xóa, Xem danh sách.
    *   *Lưu ý khi xóa:* Nếu chủ đề đang chứa từ vựng, hệ thống không xóa từ vựng mà chỉ gỡ liên kết (gán Mã chủ đề của từ vựng bằng NULL).
*   **Quản lý Từ vựng (Vocabulary):** Thêm mới (chọn chủ đề qua Dropdown/Spinner), Cập nhật, Xóa, Xem danh sách.
*   **Tra cứu và Báo cáo:**
    *   Lọc hiển thị các từ vựng theo một Chủ đề nhất định.
    *   Tìm kiếm từ vựng theo chuỗi phát âm (Ví dụ: Tra cứu các từ có phát âm bắt đầu bằng chữ "ph" - Dùng truy vấn `LIKE 'ph%'`).

## 3. Thiết kế Cơ sở dữ liệu (SQLite)
Hệ thống sử dụng **2 bảng** với **quan hệ 1-N (Một - Nhiều)**: Một chủ đề có thể có nhiều từ vựng, nhưng mỗi từ vựng chỉ thuộc 1 chủ đề tại một thời điểm.
*   **Bảng `ChuDe` (Chủ Đề):**
    *   `ma_chu_de` (TEXT): Khóa chính (Primary Key).
    *   `ten_chu_de` (TEXT): Tên chủ đề (Not Null).
*   **Bảng `TuVung` (Từ Vựng):**
    *   `ma_tu` (TEXT): Khóa chính (Primary Key).
    *   `tu`, `nghia`, `phat_am`, `vi_du` (TEXT).
    *   `ma_chu_de` (TEXT): Khóa ngoại (Foreign Key) liên kết với bảng ChuDe. Thiết lập `ON DELETE SET NULL`.
*   *Lớp thực thi:* `DatabaseHelper` (Kế thừa `SQLiteOpenHelper`), sử dụng các phương thức `onCreate()` (tạo bảng), `onUpgrade()` (nâng cấp) và các hàm CRUD (insert, update, delete, get).

## 4. Công nghệ & Giao diện (UI/UX)
*   Giao diện thiết kế theo chuẩn **Material Design** của Google.
*   Sử dụng **RecyclerView** kết hợp *ViewHolder Pattern* để hiển thị danh sách từ vựng/chủ đề giúp ứng dụng cuộn rất mượt, tối ưu RAM.
*   Dùng **CardView** (hiệu ứng thẻ), **Floating Action Button (FAB)** (nút nổi ở góc) và **Custom Dialog** (Hộp thoại) để thêm/sửa mà không phải chuyển trang.
*   Xử lý lỗi (Exception): Đã bắt các lỗi như bỏ trống trường dữ liệu, nhập trùng mã (hiển thị thông báo Toast/Alert thay vì bị crash ứng dụng).

## 5. Kết luận, Hạn chế và Hướng phát triển
> [!WARNING]
> Giám khảo thường hay đặt câu hỏi về các hạn chế này để xem sinh viên có tư duy mở rộng hay không.

*   **Hạn chế 1 (Lưu trữ cục bộ):** Dữ liệu chỉ nằm trên máy, xóa app sẽ mất dữ liệu. 
    *   *Hướng khắc phục:* Tích hợp Server API, Firebase Realtime Database để đồng bộ lên Cloud.
*   **Hạn chế 2 (Chỉ có văn bản):** Chưa có âm thanh phát âm. 
    *   *Hướng khắc phục:* Tích hợp Text-To-Speech (TTS) của Android.
*   **Hạn chế 3 (Thiếu tính năng ôn tập):** Chỉ mới là sổ tay ghi chép. 
    *   *Hướng khắc phục:* Thêm các Mini-game, Flashcard lật thẻ hoặc thuật toán Lặp lại ngắt quãng (Spaced Repetition) để tự động nhắc lại từ vựng cũ.

> [!TIP]
> **Câu hỏi thường gặp:** Giám khảo sẽ thường tập trung hỏi vào **cấu trúc CSDL (có mấy bảng, quan hệ thế nào)**, **cách bạn lấy dữ liệu lên danh sách (RecyclerView, Adapter)**, cũng như **hướng xử lý nếu mở rộng ứng dụng**. Hãy ôn kỹ các phần này!
