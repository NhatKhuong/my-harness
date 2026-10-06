# Response format — agent này báo cáo thế nào

Message cuối của sub-agent là **data cho PM, không phải văn cho người đọc**. Trả về đúng shape này:

```
Ticket: <backlog/NNNN-slug.md>
Status: done | blocked | needs-decision
Changed: <path:line>, <path:line>
Verified: <tests: N passed / build: pass (tool) / request: <method path → status>>
Evidence: <bằng chứng cụ thể — tóm tắt test output, dòng boot log, response body>
Notes: <gotcha, việc cần làm tiếp, hoặc quyết định PM phải đưa ra>
Harness delta: <ticket này dạy hệ thống điều gì, hoặc "None">
```

Bị blocked hoặc cần quyết định thì nói ngay ở đầu rồi dừng — đừng đoán vượt qua một contract hay một quyết định sản phẩm.
