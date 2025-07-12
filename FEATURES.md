# 🚀 Các chức năng của dự án DevShare Lite

## Chức năng đăng nhập
![Người dùng có thể đăng nhập nếu đã có tài khoản](./screenshots/login.png)

## Chức năng đăng ký
![Nếu chưa có tài khoản, người dùng có thể đăng ký](./screenshots/signup.png)

## Chức năng đăng xuất
![Người dùng có thể đăng xuất khi nhấn nút "Logout"](./screenshots/logout.png)
**Kết quả**: ![Kết quả khi Logout](./screenshots/logout_result.png)

## Trở về trang chủ (HomePage)
![Trở về trang chủ khi nhấn vào nút "Trang chủ"](./screenshots/home_page.png)

## Đi đến diễn đàn chứa tất cả các bài đăng (AllPosts)
![Nhấn vào nút "Diễn đàn" sẽ đưa người dùng đến trang chứa tất cả bài đăng](./screenshots/all-posts.png)
**Chú ý**: ![Có thể chuyển đến trang chuyên mục của bài viết bằng cách nhấn vào phần chuyên mục ở dưới tiêu đều, ví dụ: "#Khác"](./screenshots/route_category.png)

## Đăng bài viết (post)
![Vào trang đăng bài viết khi nhấn nút "Đăng bài", "Vào trang đăng bài", "Bắt đầu đăng bài"](./screenshots/post.png)

## Nhảy đến trang bài viết đã đăng hoặc lưu nháp
![Tự động nhảy trang khi nhấn nút "Đăng bài" hoặc "Lưu nháp" kèm với thông báo nổi trên màn hình](./screenshots/post_published_drafted.png)

## Bình luận
![Bình luận vào bài viết](./screenshots/comment.png)
**Kết quả**: ![Kết quả](./screenshots/comment_result.png)

## Trả lời bình luận
![Nhấn nút "Trả lời" để trả lời bình luận](./screenshots/comment_response.png)
**Kết quả**: ![Kết quả khi trả lời bình luận](./screenshots/comment_response_result.png)

## Chuyển đến trang của chuyên mục cụ thể
![Lựa chọn chuyên mục ở trang chủ, hoặc nhấn vào nút "Chuyên mục", sau đó lựa chọn chuyên mục muốn xem](./screenshots/category.png)
**Kết quả**: ![Chuyên mục cụ thể kèm theo các bài viết](./screenshots/category_posts.png)

## Thành viên của diễn đàn
![Hiển thị danh sách thành viên diễn đàn khi nhấn vào nút "Thành viên"](./screenshots/members.png)

## Tìm kiếm bài viết
![Tìm kiếm bằng cách nhập nội dung vào thanh tìm kiếm rồi Enter](./screenshots/search.png)
**Kết quả**: ![Kết quả](./screenshots/search_result.png)

## Hiển thị hồ sơ người dùng
![Nhấn nút "Hồ sơ" để xem hồ sơ người dùng](./screenshots/profile.png)
**Kết quả**: ![Xem hồ sơ người dùng](./screenshots/profile_view.png)

## Xem các bài đã đăng và bài nháp của người dùng
![Nhấn nút "Posts"](./screenshots/user_posts.png)

## Chỉnh sửa thông tin người dùng
![Nhấn nút "Sửa thông tin" ở phần About](./screenshots/profile_update.png)
*Trong ảnh*: Người dùng đang có tên là Đỗ Hoàng Nam và About me: Không có mô tả nào
![Chỉnh sửa thông tin](./screenshots/profile_update_info.png)
**Kết quả**: ![Đã đổi thông tin](./screenshots/profile_update_info_result.png)

Dưới đây là phần **mở rộng hoàn chỉnh** để bạn chèn **vào cuối file `FEATURES.md`** — giữ nguyên định dạng Markdown để đồng bộ với phần trước:

---

### 🚀 **Chức năng nâng cao đã thực hiện**

* Trang hiển thị **danh sách thành viên** diễn đàn.
* Cho phép **chỉnh sửa thông tin hồ sơ người dùng** (tên, địa chỉ, giới thiệu).
* Phân chia rõ bài viết thành 2 loại: **đã đăng** và **đang lưu nháp**.
* Tự động **chuyển hướng** sau khi đăng bài hoặc lưu nháp, kèm theo **thông báo nổi**.
* Hệ thống **phân trang** bài viết.
* Có thể **truy cập bài viết qua chuyên mục**
* Tự động **cập nhật các bài viết nổi bật ở trang chủ**

---

### 🛠️ Các vấn đề gặp phải & Giải pháp
*Vấn đề*: Chuyển hướng không đúng sau khi người dùng đăng/lưu bài
*Giải pháp*: Sử dụng useNavigate() từ React Router để điều hướng chính xác sau khi hoàn thành thao tác, đồng thời cập nhật trạng thái UI để phản ánh thay đổi.

*Vấn đề*: Bình luận không hiển thị ngay sau khi người dùng gửi
*Giải pháp*: Thêm logic re-fetch lại dữ liệu bình luận sau khi gửi thành công. Sử dụng useEffect hoặc cập nhật state để trigger render lại danh sách bình luận.

*Vấn đề*: Thông tin hồ sơ không được cập nhật sau khi chỉnh sửa
*Giải pháp*: Sau khi gọi API cập nhật thành công, tiến hành cập nhật lại local state hoặc gọi lại API để lấy dữ liệu mới nhất và hiển thị lại trên UI.

*Vấn đề*: Tìm kiếm bài viết trả về kết quả chưa chính xác (do còn chứa Markdown hoặc không khớp từ khóa)
*Giải pháp*: Cải tiến thuật toán tìm kiếm bằng cách loại bỏ định dạng Markdown, chuyển toàn bộ nội dung và từ khóa về dạng lowercase, và tìm khớp cả ở tiêu đề lẫn nội dung bài viết.

---

### ⚠️ **Giới hạn hiện tại của sản phẩm**

* Chưa có hệ thống **like/vote** cho bài viết và bình luận.
* Chưa có **thông báo (notification)** khi có người bình luận hoặc trả lời.
* Phản hồi bình luận **chỉ 1 cấp** (không hỗ trợ phản hồi lồng nhiều cấp).
* Chưa có **quản trị viên (admin)** để kiểm duyệt bài viết.
* Người dùng không thể **gắn ảnh/định dạng nâng cao** trong bài viết.

---

### 🌱 **Định hướng phát triển tương lai**

* Thêm hệ thống **thích (Like)/không thích (Dislike)** bài viết và bình luận.
* Tích hợp **Markdown Preview**, hỗ trợ **chèn ảnh** và **định dạng nâng cao**.
* Xây dựng hệ thống **thông báo** khi có tương tác (ai đó bình luận bài viết của mình…).
* Bổ sung trang **quản trị admin** để duyệt bài viết, chặn người dùng vi phạm.
* Tăng cường **phân quyền**: phân biệt rõ `admin`, `mod`, `người dùng thường`.
* **Cải thiện giao diện mobile** và tăng khả năng truy cập (accessibility).

---