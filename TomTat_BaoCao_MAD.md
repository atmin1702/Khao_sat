# Tóm tắt Báo cáo: Quản lý ứng dụng học từ vựng và Kiến thức môn MAD

Tóm tắt các thông tin quan trọng nhất từ file báo cáo phục vụ cho việc ôn tập và bảo vệ bài tập lớn, kèm theo kiến thức trọng tâm của môn học.

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

---

## 6. Một số kiến thức cốt lõi môn MAD (Thường hỏi khi vấn đáp)

> [!NOTE]
> Đây là các khái niệm lý thuyết nền tảng về lập trình Android bằng Java mà giảng viên rất hay hỏi để kiểm tra kiến thức nền của sinh viên, ngoài việc chỉ hỏi về chức năng của ứng dụng.

### 6.1. Vòng đời của Activity (Activity Lifecycle)
Activity là một thành phần ứng dụng cung cấp màn hình để người dùng tương tác. Vòng đời của Activity có các hàm callback chính:
*   `onCreate()`: Khởi tạo Activity, nạp giao diện (setContentView), thiết lập các biến... Gọi 1 lần duy nhất khi Activity được tạo.
*   `onStart()`: Activity bắt đầu hiển thị cho người dùng (tuy nhiên chưa thể tương tác được).
*   `onResume()`: Activity đang ở trạng thái Foreground, người dùng bắt đầu có thể tương tác (click, vuốt...).
*   `onPause()`: Activity bị che khuất một phần (vd: có một dialog nhỏ hiện lên), tạm dừng các hoạt động tốn tài nguyên.
*   `onStop()`: Activity bị che khuất hoàn toàn (vd: mở một Activity khác hoặc ấn Home).
*   `onDestroy()`: Activity bị hủy (kết thúc hoàn toàn khỏi bộ nhớ RAM).

### 6.2. Intent (Ý định)
Intent là cơ chế truyền thông điệp giữa các thành phần của Android (như chuyển trang, truyền dữ liệu). Có 2 loại:
*   **Explicit Intent (Intent tường minh):** Chỉ định rõ ràng lớp (class) nào sẽ nhận và xử lý. (Ví dụ: Dùng Intent để chuyển thẳng từ màn hình `TopicActivity` sang `VocabularyActivity`).
*   **Implicit Intent (Intent không tường minh):** Không chỉ định rõ tên lớp nhận, mà chỉ khai báo "hành động" cần thực hiện. (Ví dụ: Yêu cầu mở trình duyệt web, mở ứng dụng gọi điện, chia sẻ chữ/ảnh).

### 6.3. Phân biệt ListView và RecyclerView
*   **ListView:** Chế độ cũ, dễ sử dụng cho danh sách ngắn. Nhược điểm là tạo ra View mới liên tục khi người dùng cuộn, gây tốn RAM và giật lag nếu danh sách quá lớn.
*   **RecyclerView:** Thế hệ mới mạnh mẽ hơn, bắt buộc phải sử dụng **ViewHolder Pattern**. Khi người dùng cuộn màn hình, các View bị khuất sẽ không bị xóa mà được "tái sử dụng" (Recycle) để hiển thị dữ liệu mới đi lên. Điều này giúp cuộn mượt mà hàng ngàn item mà không bị tràn RAM.

### 6.4. SQLite và các đối tượng liên quan
*   **SQLiteOpenHelper:** Lớp abstract giúp tạo và nâng cấp database. Phải viết đè (override) 2 hàm: `onCreate()` (chạy khi DB chưa tồn tại để tạo bảng) và `onUpgrade()` (chạy khi version DB thay đổi).
*   **SQLiteDatabase:** Đối tượng chính dùng để thực thi các câu lệnh SQL (`insert()`, `update()`, `delete()`, `rawQuery()`).
*   **Cursor:** Đối tượng chứa tập kết quả trả về từ câu truy vấn (SELECT). Dùng vòng lặp `while (cursor.moveToNext())` để duyệt qua từng dòng dữ liệu.
*   **ContentValues:** Đối tượng dạng Key-Value (Khóa-Giá trị) dùng để đóng gói dữ liệu an toàn trước khi gọi hàm `insert` hoặc `update` vào bảng (tránh lỗi SQL Injection so với việc tự nối chuỗi SQL).

### 6.5. Các loại Layout cơ bản
*   **LinearLayout:** Sắp xếp các view con tuần tự theo 1 chiều duy nhất: ngang (horizontal) hoặc dọc (vertical).
*   **RelativeLayout:** Sắp xếp các view con dựa trên vị trí tương đối với nhau hoặc với layout cha (VD: nằm dưới button A, căn lề phải màn hình).
*   **ConstraintLayout:** Layout hiện đại nhất, sử dụng các "ràng buộc" (constraints) kéo thả, giúp thiết kế giao diện phức tạp mà không cần lồng ghép nhiều layout, tối ưu hiệu năng hiển thị (render).
