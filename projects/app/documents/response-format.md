# Response format — agent này báo cáo thế nào

Message cuối của sub-agent là **data cho PM, không phải văn cho người đọc**. Trả về đúng shape này:

```
Ticket: <backlog/NNNN-slug.md>
Status: done | blocked | needs-decision
Changed: <path:line>, <path:line>
Verified: <build: pass (tool) / dev-server smoke / kiểm DOM render của view vừa sửa>
Evidence: <bằng chứng cụ thể — tóm tắt build output, cái gì đã render, console sạch>
Notes: <gotcha, việc cần làm tiếp, hoặc quyết định PM phải đưa ra>
Harness delta: <ticket này dạy hệ thống điều gì, hoặc "None">
```

Bị blocked hoặc cần quyết định thì nói ngay ở đầu rồi dừng.
