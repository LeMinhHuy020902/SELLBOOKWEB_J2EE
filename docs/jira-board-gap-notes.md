# Ghi chú lệch Jira SWB01 ↔ codebase (ưu tiên chỉnh mô tả trên Jira)

**Repo đẩy code:** [LeMinhHuy020902/SELLBOOKWEB_J2EE](https://github.com/LeMinhHuy020902/SELLBOOKWEB_J2EE) (nhánh `kethop`).  
**Remote `origin` (không đổi URL):** [8DTD8/SellBookWeb](https://github.com/8DTD8/SellBookWeb) — mọi thao tác push cho board/Jira dùng remote `j2ee`, **không** `git push origin` cho tác vụ này.

Bản căn chỉnh chi tiết theo issue nằm trong [`JIRA_BOARD_ALIGNMENT_SWB01.md`](JIRA_BOARD_ALIGNMENT_SWB01.md). File này chỉ tóm **khoảng trống** đáng cập nhật trên Jira (hoặc lên kế hoạch dev sau), **không** yêu cầu sửa code chỉ để “khớp chữ” trên board.

---

## 1. Công nghệ dữ liệu (SQL vs MongoDB)

| Issue / chủ đề | Board thường ghi | Thực tế |
|----------------|------------------|---------|
| SWB01-40, SWB01-42, SWB01-58, SWB01-61 | SQL, `LIKE`, ràng buộc SQL | Dữ liệu qua **MongoDB** (ví dụ `BookRepository.findByTitleContaining`). Nên đổi acceptance criteria trên Jira thành “truy vấn MongoDB tương đương LIKE / aggregation”. |

---

## 2. Tên dự án “J2EE”

| Chủ đề | Ghi chú |
|--------|---------|
| Tên repo `SELLBOOKWEB_J2EE` | Stack hiện tại: **Spring Boot**, **MongoDB**, frontend JS/HTML. Không phải ứng dụng J2EE cổ điển. Nên thêm comment trên Epic hoặc đổi wording mô tả cho khớp. |

---

## 3. BI / biểu đồ doanh thu (SWB01-62)

| Issue | Board | Thực tế (rà nhanh frontend) |
|-------|--------|-------------------------------|
| SWB01-62 | Chart.js, biểu đồ doanh thu | **Không** thấy tham chiếu `Chart.js` / `chart.js` trong `SellBookWeb-frontend` (`.js`, `.html`). |

**Gợi ý Jira:** Đánh dấu “chưa làm” / đổi AC hoặc mở task dev riêng khi sẽ tích hợp Chart.js (hoặc thư viện khác) — tránh coi ticket là Done nếu chưa có biểu đồ trong repo.

---

## 4. Email xác nhận đơn (SWB01-50 và EPIC-05 liên quan)

| Chủ đề | Ghi chú |
|--------|---------|
| Gửi email | `AuthService` dùng **JavaMail** cho **OTP quên mật khẩu**. `OrderService.createOrder` **không** gọi gửi email xác nhận đơn (có `NotificationService` cho luồng trong app, không thay thế email đơn hàng). |

**Gợi ý Jira:** Nếu ticket mô tả “email xác nhận sau khi đặt hàng”, hoặc đổi scope thành “thông báo trong hệ thống”, hoặc giữ scope và lên backlog dev cho tích hợp mail đơn hàng.

---

## 5. CI/CD & nhánh (EPIC-10)

| Chủ đề | Ghi chú |
|--------|---------|
| Workflow | Sau merge từ `j2ee/main`, repo có [`.github/workflows/ci.yml`](../.github/workflows/ci.yml). Board SWB01-69/70 đã có minh chứng trong lịch sử commit trên GitHub. |

---

## 6. Quy ước commit gắn Jira

Khi commit sau này, dùng tiền tố issue, ví dụ: `SWB01-11: ...` để Jira/GitHub integration nhận diện (theo cấu hình dự án).

---

*Cập nhật cùng quá trình đẩy nhánh `kethop` lên remote `j2ee`, không thay đổi URL remote `origin`.*
