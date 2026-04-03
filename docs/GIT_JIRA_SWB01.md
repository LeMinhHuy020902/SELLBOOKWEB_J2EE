# Git & Jira SWB01 — Quy ước cho team

**Project key Jira:** `SWB01`

## Quy ước commit

- **Subject:** bắt đầu bằng issue key + mô tả ngắn (ASCII hoặc Unicode đều được).
  - Ví dụ: `SWB01-31 Thiết kế UI form đăng ký`
- **Ưu tiên gắn Task** (SWB01-31 … SWB01-70) khi commit thuộc một task cụ thể.
- **Story** (SWB01-11 … SWB01-30) dùng khi một commit gộp toàn bộ công việc của story hoặc không tách được task.
- Không cần ghi `[EPIC-01]` trong message; Jira nhận diện qua `SWB01-<số>`.

## Bảng Epic → Story / Task (tham chiếu)

| Epic | Stories (rút gọn) | Tasks |
|------|-------------------|-------|
| SWB01-1 | SWB01-11, SWB01-12 | SWB01-31 … SWB01-34 |
| SWB01-2 | SWB01-13, SWB01-14 | SWB01-35 … SWB01-38 |
| SWB01-3 | SWB01-15, SWB01-16 | SWB01-39 … SWB01-42 |
| SWB01-4 | SWB01-17, SWB01-18 | SWB01-43 … SWB01-46 |
| SWB01-5 | SWB01-19, SWB01-20 | SWB01-47 … SWB01-50 |
| SWB01-6 | SWB01-21, SWB01-22 | SWB01-51 … SWB01-54 |
| SWB01-7 | SWB01-23, SWB01-24 | SWB01-55 … SWB01-58 |
| SWB01-8 | SWB01-25, SWB01-26 | SWB01-59 … SWB01-62 |
| SWB01-9 | SWB01-27, SWB01-28 | SWB01-63 … SWB01-66 |
| SWB01-10 | SWB01-29, SWB01-30 | SWB01-67 … SWB01-70 |

## Nhánh `main` và `develop` (SWB01-67)

- **main:** nhánh phát hành ổn định; merge từ PR sau review.
- **develop:** nhánh tích hợp tính năng đang phát triển (tùy team; có thể đồng bộ với GitHub sau khi push).
- **Feature:** `feature/SWB01-XX-mo-ta` (theo Story hoặc Task đang làm).

Sau khi clone, tạo nhánh develop cục bộ (nếu chưa có trên remote):

```bash
git branch develop
git push -u origin develop
```

Remote dự án môn học (nếu dùng): thêm `sellbook-j2ee` trỏ tới `https://github.com/LeMinhHuy020902/SELLBOOKWEB_J2EE.git` rồi `git push -u sellbook-j2ee main` (hoặc `develop`) khi sẵn sàng.

## Nhánh feature & Pull Request

- Pattern: `feature/SWB01-XX-mo-ta-ngan` (XX = Story hoặc Task).
- **PR title** cùng format với commit: `SWB01-11 Dang ky tai khoan nguoi dung`.
- Một PR thường gắn một Story hoặc một nhóm Task trong cùng Story; squash merge vẫn giữ key trong message squash.

## Kết nối Jira với GitHub (SWB01-68)

1. Jira **Settings → Apps → Explore more apps** → cài **GitHub for Jira** (Atlassian).
2. Trong app: **Add organization** / **Connect repository** → chọn org/user chứa `SELLBOOKWEB_J2EE`.
3. Mỗi commit/PR có key `SWB01-XX` trong subject hoặc mô tả → tab **Development** trên issue Jira hiển thị liên kết sau vài phút.

Kiểm tra: push một commit có `SWB01-68` trong message → mở issue SWB01-68 trên Jira xem đã gắn commit chưa.
