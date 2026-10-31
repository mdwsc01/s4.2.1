# BÀI TẬP: PHÂN BIỆT REST VÀ SOAP – THIẾT KẾ API RESTFUL

## 1. Mục tiêu

- Phân biệt được kiến trúc REST và giao thức SOAP.
- Áp dụng quy tắc đặt tên endpoint RESTful khi thiết kế API quản lý nhân viên.

## 2. Phần 1 – Lý thuyết: Vì sao chọn REST thay vì SOAP?

Đối với ứng dụng Quản lý nhân sự, em chọn kiến trúc **REST** thay vì SOAP vì REST linh hoạt, dễ triển khai và phù hợp với ứng dụng web. REST sử dụng trực tiếp các phương thức HTTP phổ biến như GET, POST, PUT và DELETE để thao tác với tài nguyên. Dữ liệu giữa Client và Server thường được trao đổi dưới dạng **JSON**, có cấu trúc gọn nhẹ và dễ xử lý trong JavaScript, Java cũng như các ứng dụng di động. Ngoài ra, REST không yêu cầu thông điệp SOAP dạng XML với cấu trúc bao bọc bắt buộc, nên có thể giảm dung lượng trao đổi dữ liệu và chi phí xử lý trong nhiều trường hợp. SOAP vẫn hữu ích ở những hệ thống cần các tiêu chuẩn như WS-Security hoặc quy trình giao dịch phức tạp, nhưng đối với các chức năng quản lý nhân viên thông thường, REST đơn giản hơn và thuận tiện cho việc phát triển, bảo trì, mở rộng.

### So sánh ngắn gọn

| Tiêu chí | REST | SOAP |
|---|---|---|
| Bản chất | Phong cách kiến trúc | Giao thức trao đổi thông điệp |
| Định dạng dữ liệu | Thường dùng JSON; cũng có thể dùng XML, v.v. | Thông điệp XML theo chuẩn SOAP |
| Thao tác | Tận dụng các HTTP Method (GET, POST, PUT, DELETE, …) | Xác định thao tác trong thông điệp SOAP |
| Độ phức tạp | Thường đơn giản, phù hợp API web | Cấu trúc và tiêu chuẩn thường phức tạp hơn |
| Phù hợp | Web, mobile, API quản lý tài nguyên | Một số hệ thống doanh nghiệp cần chuẩn WS-* |

## 3. Phần 2 – Thiết kế API quản lý nhân viên (Employee)

Tài nguyên cần quản lý là **Employee**. Theo quy tắc RESTful, URL sử dụng **danh từ số nhiều** `employees`, không dùng động từ như `getAllEmployees`, `addEmployee` hay `deleteEmployee` trong đường dẫn. Hành động được thể hiện bằng HTTP Method.

**Base URL minh họa:** `http://localhost:8080`

| STT | Chức năng | HTTP Method | Endpoint | Ý nghĩa |
|---|---|---|---|---|
| 1 | Lấy danh sách tất cả nhân viên | **GET** | `/employees` | Trả về danh sách nhân viên |
| 2 | Lấy thông tin nhân viên theo ID | **GET** | `/employees/{id}` | Trả về nhân viên có ID tương ứng |
| 3 | Tạo mới một nhân viên | **POST** | `/employees` | Nhận dữ liệu nhân viên và tạo bản ghi mới |
| 4 | Cập nhật thông tin nhân viên | **PUT** | `/employees/{id}` | Cập nhật thông tin nhân viên theo ID |
| 5 | Xóa một nhân viên | **DELETE** | `/employees/{id}` | Xóa nhân viên có ID tương ứng |

### Ví dụ Request – Response

**1. Lấy toàn bộ nhân viên**

```http
GET /employees HTTP/1.1
Host: localhost:8080
```

Ví dụ Response `200 OK`:

```json
[
  {"id": 1, "name": "Nguyen Van An", "department": "IT"},
  {"id": 2, "name": "Tran Thi Binh", "department": "HR"}
]
```

**2. Lấy nhân viên theo ID**

```http
GET /employees/1 HTTP/1.1
Host: localhost:8080
```

Ví dụ Response `200 OK`:

```json
{"id": 1, "name": "Nguyen Van An", "department": "IT"}
```

**3. Tạo nhân viên mới**

```http
POST /employees HTTP/1.1
Host: localhost:8080
Content-Type: application/json

{"name": "Le Van Cuong", "department": "Sales"}
```

Ví dụ Response `201 Created`:

```json
{"id": 3, "name": "Le Van Cuong", "department": "Sales"}
```

**4. Cập nhật nhân viên**

```http
PUT /employees/3 HTTP/1.1
Host: localhost:8080
Content-Type: application/json

{"name": "Le Van Cuong", "department": "Marketing"}
```

Ví dụ Response `200 OK`:

```json
{"id": 3, "name": "Le Van Cuong", "department": "Marketing"}
```

**5. Xóa nhân viên**

```http
DELETE /employees/3 HTTP/1.1
Host: localhost:8080
```

Ví dụ Response: `204 No Content` (xóa thành công, không trả nội dung).

**Lưu ý:** Nếu không tìm thấy nhân viên theo ID, Server có thể trả về `404 Not Found`. Các ví dụ trên là thiết kế API, không có nghĩa các endpoint đã được lập trình trong project hiện tại.

## 4. Kết luận

Thiết kế trên đáp ứng đúng nguyên tắc RESTful: dùng URL là **danh từ số nhiều** (`/employees`), dùng HTTP Method để xác định hành động, dùng `{id}` để chỉ một tài nguyên cụ thể và trao đổi dữ liệu ở dạng JSON. Nhờ đó, API có cấu trúc nhất quán, dễ hiểu và thuận tiện cho Client sử dụng.
