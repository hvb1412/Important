# 📊 DATABASE DESIGN – DevShare Lite

## 🧠 Mục tiêu thiết kế

Cơ sở dữ liệu của **DevShare Lite** được thiết kế nhằm hỗ trợ một diễn đàn chia sẻ bài viết, thảo luận kỹ thuật, nơi người dùng có thể tạo tài khoản, viết bài, gắn thẻ, bình luận và phân loại nội dung theo chuyên mục. Hệ thống ưu tiên khả năng mở rộng, tối ưu truy vấn và đảm bảo tính toàn vẹn dữ liệu.

---

## 🔗 Sơ đồ mối quan hệ (ER Diagram - mô tả text)

![Sơ đồ quan hệ của Database DevShare Lite](./images/Sơ-đồ-ERD-Database.png)

---

## 📁 Chi tiết các bảng

### 1. `users` – Người dùng
| Tên cột      | Kiểu dữ liệu     | Ràng buộc                     | Mô tả                      |
|--------------|------------------|-------------------------------|----------------------------|
| user_id      | SERIAL           | PRIMARY KEY                   | ID người dùng              |
| user_name    | VARCHAR(100)     | NOT NULL                      | Tên hiển thị người dùng    |
| email        | VARCHAR(255)     | NOT NULL, UNIQUE              | Email đăng ký              |
| password     | TEXT             | NOT NULL                      | Mật khẩu đã mã hóa         |
| role         | VARCHAR(50)      | NOT NULL, DEFAULT 'member'    | Quyền: admin, member.      |
| created_at   | TIMESTAMP        | DEFAULT CURRENT_TIMESTAMP     | Thời gian tạo tài khoản    |
| updated_at   | TIMESTAMP        | DEFAULT CURRENT_TIMESTAMP     | Lần cập nhật cuối          |

---

### 2. `user_profile` – Hồ sơ người dùng
| Tên cột      | Kiểu dữ liệu     | Ràng buộc                         | Mô tả                   |
|--------------|------------------|-----------------------------------|-------------------------|
| user_id      | INT              | PRIMARY KEY, FK → users(user_id)  | ID người dùng           |
| user_name    | VARCHAR(100)     | NOT NULL                          | Tên hiển thị            |
| image        | VARCHAR(255)     | NULLABLE                          | Ảnh đại diện            |
| bio          | TEXT             | NULLABLE                          | Giới thiệu ngắn         |
| address      | VARCHAR(255)     | NULLABLE                          | Địa chỉ                 |
| phone_number | VARCHAR(50)      | NULLABLE                          | Số điện thoại           |
| created_at   | TIMESTAMP        | DEFAULT CURRENT_TIMESTAMP         | Ngày tạo hồ sơ          |
| updated_at   | TIMESTAMP        | DEFAULT CURRENT_TIMESTAMP         | Ngày cập nhật           |

---

### 3. `categories` – Chuyên mục
| Tên cột      | Kiểu dữ liệu     | Ràng buộc      | Mô tả                           |
|--------------|------------------|----------------|---------------------------------|
| category_id  | SERIAL           | PRIMARY KEY    | ID chuyên mục                   |
| name         | VARCHAR(100)     |                | Tên chuyên mục                  |
| description  | TEXT             |                | Mô tả về chuyên mục             |

---

### 4. `posts` – Bài viết
| Tên cột      | Kiểu dữ liệu     | Ràng buộc                                        | Mô tả                    |
|--------------|------------------|--------------------------------------------------|--------------------------|
| post_id      | SERIAL           | PRIMARY KEY                                      | ID bài viết              |
| user_id      | INT              | FK → users(user_id), ON DELETE SET NULL          | Tác giả                  |
| title        | VARCHAR(255)     |                                                  | Tiêu đề bài viết         |
| content      | TEXT             |                                                  | Nội dung chính           |
| category_id  | INT              | FK → categories(category_id), ON DELETE SET NULL | Chuyên mục chính         |
| status       | VARCHAR(50)      | CHECK (status IN ('draft', 'published'))         | Trạng thái               |
| created_at   | TIMESTAMP        | DEFAULT CURRENT_TIMESTAMP                        | Ngày tạo                 |
| updated_at   | TIMESTAMP        | DEFAULT CURRENT_TIMESTAMP                        | Ngày cập nhật            |

---

### 5. `comments` – Bình luận
| Tên cột      | Kiểu dữ liệu     | Ràng buộc                                      | Mô tả                      |
|--------------|------------------|------------------------------------------------|----------------------------|
| comment_id   | SERIAL           | PRIMARY KEY                                    | ID bình luận               |
| post_id      | INT              | FK → posts(post_id), ON DELETE CASCADE         | Bài viết liên quan         |
| user_id      | INT              | FK → users(user_id), ON DELETE CASCADE         | Người bình luận            |
| content      | TEXT             |                                                | Nội dung bình luận         |
| parent_id    | INT              | FK → comments(comment_id), ON DELETE CASCADE   | ID bình luận cha (nếu có)  |
| created_at   | TIMESTAMP        | DEFAULT CURRENT_TIMESTAMP                      | Ngày tạo                   |
| updated_at   | TIMESTAMP        | DEFAULT CURRENT_TIMESTAMP                      | Ngày cập nhật              |

---

### 6. `post_categories` – Liên kết bài viết và chuyên mục (nhiều - nhiều)
| Cột          | Kiểu dữ liệu     | Ràng buộc                                     | Mô tả                      |
|--------------|------------------|-----------------------------------------------|----------------------------|
| post_id      | INT              | PRIMARY KEY, FK → posts(post_id)              | ID bài viết                |
| category_id  | INT              | PRIMARY KEY, FK → categories(category_id)     | ID chuyên mục              |

> Lưu ý: `posts.category_id` là chuyên mục **chính**, còn bảng này là các chuyên mục **phụ liên quan**.

---

### 7. `tags` – Thẻ bài viết
| Cột         | Kiểu dữ liệu     | Ràng buộc      | Mô tả                      |
|-------------|------------------|----------------|----------------------------|
| tag_id      | SERIAL           | PRIMARY KEY    | ID thẻ                     |
| name        | VARCHAR(100)     |                | Tên thẻ (tag)              |

---

### 8. `post_tags` – Gắn thẻ cho bài viết (nhiều - nhiều)
| Cột         | Kiểu dữ liệu     | Ràng buộc                                 | Mô tả                |
|-------------|------------------|-------------------------------------------|----------------------|
| post_id     | INT              | PRIMARY KEY, FK → posts(post_id)          | ID bài viết          |
| tag_id      | INT              | PRIMARY KEY, FK → tags(tag_id)            | ID thẻ               |

---

## ✅ Các ràng buộc quan trọng

- **ON DELETE CASCADE**: được sử dụng với bình luận, bài viết, user_profile, để đảm bảo dữ liệu phụ thuộc bị xóa theo.
- **ON DELETE SET NULL**: với bài viết khi user hoặc category bị xóa, tránh mất dữ liệu chính.

---

## 🔄 Mối quan hệ giữa các bảng (1-1, 1-N, N-N)

users           1 ────── 1 user_profile
users           1 ──────< posts
users           1 ──────< comments
posts           1 ──────< comments
comments        1 ──────< comments (self-reference)
posts           N ──────< post_categories >────── N categories
posts           N ──────< post_tags >────── N tags

---

## 🚀 Dự định cải tiến trong tương lai

- Thêm bảng `likes` để người dùng thích bài viết hoặc bình luận.
- Thêm bảng `notifications` để theo dõi tương tác.
- Thêm trường `slug` trong `posts` để tạo URL thân thiện.

---

## 🧩 Lý do lựa chọn cơ sở dữ liệu quan hệ (PostgreSQL)
### DevShare Lite được triển khai trên hệ quản trị cơ sở dữ liệu quan hệ PostgreSQL, vì các lý do:

- Dữ liệu có cấu trúc rõ ràng: Các thực thể như người dùng, bài viết, chuyên mục… có quan hệ ràng buộc rõ ràng, phù hợp mô hình quan hệ.
- Hỗ trợ mạnh mẽ về ràng buộc toàn vẹn (constraints): như FOREIGN KEY, ON DELETE CASCADE, giúp đảm bảo tính toàn vẹn dữ liệu.
- Khả năng mở rộng và hiệu suất tốt: PostgreSQL hỗ trợ index, query optimizer mạnh, phù hợp với các ứng dụng vừa và lớn.
- Cần giao dịch (ACID): Tính nhất quán dữ liệu là ưu tiên trong hệ thống có tương tác như bình luận, thích, cập nhật bài viết.

