- Phần 1: Thiết kế các endpoint
1. Lấy danh sách tất cả sách
   HTTP method: GET
Đường dẫn: /books
Mã thành công: 200 OK
Mã lỗi:
500 Internal Server Error – Lỗi server
403 Forbidden – Không có quyền truy cập

2. Lấy chi tiết một sách theo id
   HTTP method: GET
Đường dẫn: /books/{id}
Mã thành công: 200 OK
Mã lỗi:
404 Not Found – Không tìm thấy sách có id đó
400 Bad Request – Id không hợp lệ (vd: không phải số)

3. Thêm một sách mới
   HTTP method: POST
Đường dẫn: /books
Mã thành công: 201 Created
Mã lỗi:
400 Bad Request – Thiếu hoặc sai định dạng dữ liệu
409 Conflict – Sách đã tồn tại (vd: trùng title + author)

4. Cập nhật toàn bộ thông tin sách theo id
   HTTP method: PUT
Đường dẫn: /books/{id}
Mã thành công: 200 OK
Mã lỗi:
404 Not Found – Không tìm thấy sách
400 Bad Request – Dữ liệu gửi lên không hợp lệ

5. Xóa một sách theo id
   HTTP method: DELETE
Đường dẫn: /books/{id}
Mã thành công: 200 OK (hoặc 204 No Content)
Mã lỗi:
404 Not Found – Không tìm thấy sách
403 Forbidden – Không có quyền xóa

6. Tìm sách theo tác giả
   HTTP method: GET
Đường dẫn: /books?author={authorName}
Mã thành công: 200 OK
Mã lỗi:
400 Bad Request – Tên tác giả không hợp lệ
500 Internal Server Error – Lỗi server

 - Phần 2: Ví dụ request/response (JSON)
+ Ví dụ 1: Thêm một sách mới thành công
Request:

http
POST /books
Content-Type: application/json

{
"title": "Nhà giả kim",
"author": "Paulo Coelho",
"year": 1988,
"available": true
}
Response (201 Created):

json
{
"id": 101,
"title": "Nhà giả kim",
"author": "Paulo Coelho",
"year": 1988,
"available": true,
"message": "Thêm sách thành công"
}

+ Ví dụ 2: Tìm sách theo tác giả không có kết quả
Request:

http
GET /books?author=Nguyễn%20Không%20Tồn%20Tại
Response (200 OK):

json
{
"author": "Nguyễn Không Tồn Tại",
"total": 0,
"books": [],
"message": "Không tìm thấy sách nào của tác giả này"
}


- Phần 4: Câu hỏi lý thuyết – PUT vs PATCH
Đặc điểm	                  PUT	                                                            PATCH
Mục đích	                  Cập nhật toàn bộ tài nguyên	                                    Cập nhật một phần tài nguyên
Dữ liệu gửi lên	              Toàn bộ các trường (kể cả trường không thay đổi)	                Chỉ các trường cần thay đổi
Hậu quả nếu thiếu trường	  Các trường thiếu sẽ bị ghi đè thành null hoặc giá trị mặc định	Không ảnh hưởng đến trường khác
Idempotent	                  Có (gọi nhiều lần cùng kết quả)	                                Không nhất thiết (có thể phụ thuộc thuật toán)
--> Trong bài tập cập nhật sách, nên dùng PUT hay PATCH?
-> Nên dùng PUT nếu client gửi toàn bộ thông tin sách mới nhất (dùng trong form sửa toàn bộ).
-> Nên dùng PATCH nếu chỉ cập nhật một vài trường, ví dụ chỉ đổi available từ true sang false mà không cần gửi lại title, author, year.

Trong yêu cầu "cập nhật toàn bộ thông tin sách theo id" → đúng với PUT.
Còn nếu muốn tối ưu và thực tế hơn → nên hỗ trợ cả PATCH để cập nhật linh hoạt
