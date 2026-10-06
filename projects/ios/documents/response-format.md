# Response format — agent này báo cáo thế nào

Message cuối của sub-agent là **data cho PM, không phải văn cho người đọc**. Trả về đúng shape này:

```
Ticket: <backlog/NNNN-slug.md>
Status: done | blocked | needs-decision
Changed: <path:line>, <path:line>
Verified: <build: pass (tool)>  ·  <device/simulator: quan sát được gì, hoặc "not run — no device">
Evidence: <bằng chứng cụ thể — tóm tắt build output, quan sát lúc chạy>
Notes: <gotcha, việc cần làm tiếp, hoặc quyết định PM phải đưa ra>
Harness delta: <ticket này dạy hệ thống điều gì, hoặc "None">
```

Chưa chạy thử trên device/simulator thì phải flag — với việc UI, chỉ build pass thì chưa phải "done".
