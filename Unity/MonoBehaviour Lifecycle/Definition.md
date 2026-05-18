---
note_type: definition
tags:
  - unity
sticker: lucide//star
---

## Definition
- `MonoBehaviour Lifecycle` là vòng đời callback của một `MonoBehaviour`: tạo instance, bật/tắt component, update theo frame, update physics, render, rồi hủy.
- Theo Unity `6.4`, lifecycle không chỉ là danh sách hàm. Nó là hợp đồng về **khi nào code được phép chạy** và **callback nào phù hợp với loại logic nào**.
- Nắm lifecycle giúp tránh ba lỗi lớn: đọc reference quá sớm, đặt physics sai phase, và quên cleanup khi object bị disable/destroy.

## Related notes
- [[MonoBehaviour Lifecycle]]
- [[Execution Order]]
- [[Initialization]]
- [[Update Loop]]
- [[Decommissioning]]
- [[Summary]]
