# Căn board Jira SWB01 với repo (không sửa code ứng dụng)

**Repo minh chứng:** [LeMinhHuy020902/SELLBOOKWEB_J2EE](https://github.com/LeMinhHuy020902/SELLBOOKWEB_J2EE)  
**Mục đích:** Dùng file này để cập nhật **mô tả issue**, **comment**, và **trạng thái** trên Jira cho khớp codebase thực tế (Spring Boot + MongoDB + frontend tĩnh/JS).

---

## 1. Wording toàn cục — thay “SQL” bằng “MongoDB” (copy vào Jira)

Áp dụng cho các task đang ghi SQL / LIKE / ràng buộc SQL khi thực tế dùng MongoDB:

| Issue (gợi ý) | Ghi trên board cũ | Đổi mô tả thành |
|----------------|-------------------|-----------------|
| SWB01-40 | Query SQL LIKE tìm kiếm | Tìm sách theo tên: `BookRepository` / `findByTitleContaining` (MongoDB, tương đương LIKE). API: `GET /api/books/search?title=...`. Code: `BookService.searchBooks` |
| SWB01-42 | Logic lọc server (nếu ghi SQL) | Lọc/lấy dữ liệu: MongoDB repository + service; không dùng câu SQL thuần |
| SWB01-58 | Kiểm tra ràng buộc SQL | Validation & lưu trữ MongoDB (schema/document validation tại ứng dụng hoặc collection) |
| SWB01-61 | Query SQL tổng hợp doanh thu | Báo cáo doanh thu qua API admin (aggregation MongoDB / service). Tham khảo: `GET /api/admin/reports/revenue` — `AdminReportController` |

**Ghi chú Epic:** Nếu tên Epic vẫn ghi “J2EE/SQL” thuần, có thể thêm comment: *“Stack triển khai: Spring Boot REST + MongoDB; mô tả task đã cập nhật cho khớp.”*

---

## 2. Checklist Done + tham chiếu file/API (để đánh Done trên Jira)

Sau khi đối chiếu, thêm **comment** trên từng issue: *“Done — tham chiếu repo: …”* kèm đường dẫn dưới đây.

### Epic 10 — Hạ tầng & CI/CD (SWB01-29, 30, 67–70)

| Issue | Minh chứng trong repo |
|-------|------------------------|
| SWB01-29, 30 | Cấu trúc monorepo `SellBookWeb-backend/`, `SellBookWeb-frontend/`, `docs/GIT_JIRA_SWB01.md` |
| SWB01-67–70 | `.github/workflows/ci.yml`, nhánh `main`/`develop` trên GitHub, commit message `SWB01-67` … `SWB01-70`, tab Development trên Jira |

### Epic 1 — Tài khoản (SWB01-11, 12, 31–34)

| Issue | Minh chứng |
|-------|------------|
| SWB01-11 | `AuthService.register`, `AuthController` → `POST /api/auth/register` |
| SWB01-12 | `POST /api/auth/login`, JWT, `JwtAuthenticationFilter` |
| SWB01-31 | UI: `SellBookWeb-frontend/login.html` (và flow JS tương ứng) |
| SWB01-32 | `PasswordEncoder` trong `AuthService` |
| SWB01-33 | `JwtTokenProvider`, filter JWT |
| SWB01-34 | Có test một số controller trong `SellBookWeb-backend/src/test/`. Nếu GV yêu cầu E2E login: giữ Open hoặc ghi rõ “đã có unit/controller test” trong comment |

### Epic 2 — Danh mục & sách (SWB01-13, 14, 35–38)

| Issue | Minh chứng |
|-------|------------|
| ST-03, ST-04 | `BookController` `GET /api/books`, `GET /api/books/{id}`; `CategoryController` |
| SWB01-35–38 | Grid/list + chi tiết: frontend customer; backend `getBookById` = MongoDB `findById` |

### Epic 3 — Tìm kiếm & lọc (SWB01-15, 16, 39–42)

| Issue | Minh chứng |
|-------|------------|
| SWB01-15 | `GET /api/books/search?title=` — `BookService.searchBooks` |
| SWB01-16, SWB01-41, 42 | **Lọc giá + category trên client:** `js/customer-app.js` — `filterByPrice`, `filterBooks` (khoảng giá `under50`, `50to100`, …), lọc `categoryId` sau khi đã tải `allBooks`. *Không bắt buộc API query giá server-side.* |
| SWB01-39 | Thanh tìm kiếm/header: UI customer cùng file trên |

### Epic 4 — Giỏ hàng (SWB01-17, 18, 43–46)

| Issue | Minh chứng |
|-------|------------|
| SWB01-17–18, 43–45 | API: `CartController` `/api/cart/{userId}/...`; client: `localStorage` key `cart` trong `customer-app.js` / `js/customer-app.js` |
| SWB01-44 | `CartService.addItemToCart` kiểm tra `Book` tồn tại trước khi thêm |
| SWB01-46 | **Chưa có** `CartServiceTest` chuyên biệt — xem mục 3 (thu hẹp scope) |

### Epic 5 — Thanh toán & đặt hàng (SWB01-19, 20, 47–50)

| Issue | Minh chứng |
|-------|------------|
| SWB01-47–49 | `OrderController`, `OrderService.createOrder`, validation DTO |
| SWB01-50 | **Không gửi email SMTP** — `OrderService` kết hợp `NotificationService` (thông báo in-app). Xem mục 3 |

### Epic 6 — Đơn hàng KH (SWB01-21, 22, 51–54)

| Issue | Minh chứng |
|-------|------------|
| SWB01-21, 22, 51–53 | `OrderController`, `getOrdersByUserId`, `PUT .../cancel` |
| SWB01-54 | **Hoàn kho:** `cancelOrder` hiện chỉ đổi trạng thái — không cộng lại `book.quantity` trong code đã rà soát. Xem mục 3 |

### Epic 7 — Admin kho (SWB01-23, 24, 55–58)

| Issue | Minh chứng |
|-------|------------|
| SWB01-23–24, 55–57 | `AdminBookController` / API sách, form admin frontend |
| SWB01-58 | Ràng buộc dữ liệu MongoDB / validation ứng dụng (không SQL) |

### Epic 8 — Admin bán hàng & doanh thu (SWB01-25, 26, 59–62)

| Issue | Minh chứng |
|-------|------------|
| SWB01-25, SWB01-59–60 | Quản lý đơn: `js/admin-app.js` có `loadOrders`, cập nhật trạng thái; **lưu ý:** `admin.html` đang load `admin-app.js` (root) — xem mục 4 |
| SWB01-26, SWB01-61 | `AdminReportController` — `/api/admin/reports/revenue` (và các endpoint báo cáo khác) |
| SWB01-62 | **Chưa dùng Chart.js** trên frontend đã kiểm tra — xem mục 3 |

### Epic 9 — Đánh giá (SWB01-27, 28, 63–66)

| Issue | Minh chứng |
|-------|------------|
| Toàn bộ | `ReviewController`: POST review, theo `bookId`, pending, `PUT .../approve`, `DELETE` |

---

## 3. Thu hẹp / tách scope — văn bản gợi ý cho Jira (copy comment)

Dùng khi cần **không sửa code** mà vẫn nhất quán báo cáo:

**SWB01-16 / SWB01-42 — Lọc giá & bộ lọc**  
*“Đã triển khai lọc theo thể loại và khoảng giá trên giao diện khách (`js/customer-app.js`), sau khi tải danh sách sách từ API. Không dùng API riêng lọc giá server-side.”*

**SWB01-50 — Email xác nhận**  
*“Thay vì email SMTP: hệ thống gửi thông báo trong ứng dụng qua Notification khi có đơn / đổi trạng thái (`OrderService` + `NotificationService`). Nếu môn học bắt buộc email thật, ghi lại là việc mở rộng tương lai.”*

**SWB01-54 — Hoàn kho**  
*“Hủy đơn: cập nhật trạng thái đơn qua API. Hoàn số lượng sách về kho khi hủy: chưa có trong phiên bản hiện tại — tách sang backlog hoặc ghi ‘ngoài phạm vi phiên bản này’.”*

**SWB01-46 — Unit test tổng tiền giỏ**  
*“Chưa có `CartServiceTest` riêng; có thể đánh Done với minh chứng kiểm thử thủ công / hoặc chuyển sang backlog ‘bổ sung unit test’.”*

**SWB01-62 — Chart.js doanh thu**  
*“Frontend admin hiện dashboard số liệu tổng quan; biểu đồ Chart.js chưa có. Đổi AC: báo cáo qua API + bảng/số hoặc backlog chart.”*

---

## 4. Thống nhất minh chứng Admin — `admin.html` vs hai file JS

| File | Nội dung |
|------|-----------|
| `SellBookWeb-frontend/admin.html` | Đang `<script src="admin-app.js">` — file **root** `admin-app.js` (bản rút gọn, không có khối quản lý đơn như bản đầy đủ). |
| `SellBookWeb-frontend/js/admin-app.js` | Bản **đầy đủ hơn**: có `loadOrders`, chi tiết đơn, cập nhật trạng thái, v.v. |

**Gợi ý ghi trên Jira (sprint / Epic 8):**  
*“Minh chứng quản trị đơn hàng: dùng mã nguồn `js/admin-app.js` + mô tả cách mở (hoặc trang admin tích hợp tương đương). File `admin.html` mặc định trỏ `admin-app.js` gốc — nếu báo cáo yêu cầu đúng màn đơn hàng, nêu rõ bản đầy đủ nằm ở `js/admin-app.js`.”*

*(Chỉnh một dòng script trong `admin.html` sẽ thống nhất hành vi; đó là thay đổi code — **không** nằm trong phạm vi ‘không sửa code’ của kế hoạch này.)*

---

## 5. Việc không nên làm (trung thực báo cáo)

- Không tạo commit rỗng chỉ để khớp số issue.
- Nên: chuyển trạng thái Jira đúng với tính năng có thật + link tới file/API trong comment.

---

*Tài liệu này căn theo kế hoạch ‘Jira board vs codebase’; cập nhật khi codebase thay đổi.*
