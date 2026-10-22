# ADR 0000: Các nguyên tắc vận hành nền tảng

**Trạng thái:** Đã chấp nhận (Accepted)  
**Ngày:** 2026-01-01  
**Người phụ trách:** &lt;Người phụ trách&gt;

## Bối cảnh

Đây là bản ghi nhận hồi tố về các nguyên tắc nền tảng mà workspace đang áp dụng. Mục đích là tạo cơ sở cho những quyết định sau này kế thừa hoặc thay thế các nguyên tắc hiện tại.

## Quyết định

1. **PM không bao giờ viết code triển khai.** PM chịu trách nhiệm điều phối và phân công công việc; các sub-agent chịu trách nhiệm xây dựng và triển khai. Nếu PM trực tiếp viết code, ranh giới quy ước riêng của từng bộ phận (surface) sẽ bị phá vỡ.

2. **Board được quản lý bằng file.** Ticket, bug và quyết định được lưu dưới dạng các file Markdown có đánh số trong thư mục `management/`, được lập chỉ mục thông qua `STATUS.md`. `git` đóng vai trò backend quản lý dữ liệu.

3. **Quyết định phải được ghi lại, không chỉ ghi nhớ.** Những quyết định ảnh hưởng đến nhiều thành phần được ghi thành ADR tại đây, kèm theo lý do. Khi cần thay đổi, hãy tạo quyết định thay thế thay vì chỉnh sửa nội dung ADR cũ.

4. **Thư mục `documents/` của mỗi sub-project là nguồn quy chuẩn chính thức.** Agent phụ trách sub-project phải đọc tài liệu trong thư mục này trước khi thực hiện công việc. Nếu phát hiện xung đột với yêu cầu, agent phải dừng lại thay vì tự suy đoán hoặc đưa ra quyết định tùy ý.

5. **Quy tắc đặt tên:**
   - `kebab-case` cho tên file và slug.
   - `ALL_CAPS` cho các file chỉ mục và trạng thái như `STATUS.md`, `CANDIDATES.md`.

## Các phương án đã cân nhắc

- **Sử dụng database board ngay từ đầu:** Cung cấp nhiều khả năng hơn như RBAC, giao diện web và tránh sai lệch giữa dữ liệu với file chỉ mục. Tuy nhiên, phương án này đòi hỏi hạ tầng thực tế và cơ chế quản lý tài khoản, trong khi workspace hiện chỉ phục vụ một người dùng. Vì vậy, phương án được hoãn lại cho đến khi có nhu cầu làm việc theo nhóm. Xem pm-playbook → mục "Chuyển sang sử dụng board chung cho nhóm".

- **PM vừa điều phối vừa viết code:** Có thể giúp công việc hoàn thành nhanh hơn trong ngắn hạn, nhưng sẽ xóa nhòa ranh giới giữa PM và các quy ước riêng của từng bộ phận, vốn giúp duy trì tính nhất quán trong môi trường multi-agent.

## Hệ quả

- Không cần thiết lập hạ tầng ban đầu và có đầy đủ lịch sử thay đổi thông qua `git`. Tuy nhiên, `STATUS.md` phải được cập nhật thủ công và có thể không phản ánh trạng thái mới nhất. Vì vậy, ID phải được xác định từ các thư mục chứa file thực tế, không dựa hoàn toàn vào file chỉ mục.

- Mô hình vận hành có tính linh hoạt và khả năng chuyển đổi. Khi nâng cấp sang database dùng chung, chỉ thay đổi **nơi lưu trữ board**, không thay đổi cách thức vận hành của workspace.