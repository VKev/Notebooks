---
aliases:
  - Definition
  - Draw Call Definition
  - Lệnh vẽ
note_type: permanent
created: 2026-03-27
tags:
  - unity
color: var(--mk-color-base-40)
---

## One-line answer
- Trong Unity, `draw call` là lúc CPU chuẩn bị render state rồi gửi lệnh cho GPU vẽ một `mesh`; mặc định mỗi `mesh` cần draw call riêng.

## Problem
- Khi scene có quá nhiều draw call, CPU có thể tốn nhiều thời gian chỉ để chuẩn bị dữ liệu render thay vì làm gameplay hay simulation.
- Nếu bottleneck nằm ở CPU render setup, frame time sẽ tăng dù GPU chưa chắc đã quá tải.

## Core idea
- Draw call optimization chủ yếu là giảm số lần CPU phải đổi render state và submit lệnh cho GPU.
- Vì vậy, thứ cần nhìn không chỉ là số object, mà là:
  - số draw call hoặc `Batches`
  - số lần đổi shader pass, thường thấy qua `SetPass`
  - scene có đang `CPU-bound` ở rendering hay không

## Mechanism
- Unity docs mô tả draw call có hai bước chính:
  - CPU cập nhật render state, như shader, texture, và buffer
  - CPU submit draw call để GPU biết phải vẽ gì
- Phần update render state là phần tốn CPU nhất.
- Nếu nhiều object dùng cùng render state, Unity có thể gom chúng lại bằng batching, instancing, hoặc SRP-side optimization để giảm overhead.
- Trong `Rendering Statistics`:
  - `Batches` là tổng số draw call batch Unity xử lý trong frame, gồm static, dynamic, và instance batches
  - `Saved by batching` là số draw call Unity đã gộp được
  - `SetPass` là số lần Unity đổi shader pass trong frame, thường phản ánh render-state overhead rõ hơn

## When to use
- Dùng note này khi bạn cần mental model trước khi đọc `Stats`, `Profiler`, hoặc `Frame Debugger`.
- Hợp khi bạn đang cố hiểu vì sao scene có nhiều `Batches` hoặc `SetPass`.

## Benefits
- Giúp phân biệt rõ `draw call`, `batch`, và `SetPass`.
- Giúp tránh tối ưu mù quáng chỉ vì thấy scene có nhiều object.
- Tạo nền để chọn đúng tool như `SRP Batcher`, `GPU Instancing`, hay `Batching`.

## Trade-offs / Limits
- Ít draw call hơn không tự động có nghĩa là nhanh hơn nếu bottleneck thực sự nằm ở GPU, fill rate, shader cost, hoặc memory.
- Một số kỹ thuật giảm draw call đổi lại bằng memory cao hơn hoặc culling kém hơn.

## Common mistakes
- Nghĩ rằng draw call chỉ là vấn đề phía GPU.
- Tối ưu draw call khi scene thực ra đang `GPU-bound`.
- Chỉ nhìn `Batches` mà bỏ qua `SetPass` và render CPU time.
- Nghĩ rằng `SRP Batcher` luôn làm số draw call giảm mạnh; thực tế nó chủ yếu giảm state-change overhead.

## Example
- Hai object dùng cùng `material` có cơ hội được tối ưu cùng draw path.
- Hai object dùng shader pass hoặc material khác nhau thường buộc Unity đổi render state thêm lần nữa.

```csharp
// Same mesh, same material, many copies:
// co the hop voi GPU instancing

// Different materials or shader variants:
// thuong lam Unity tang them state setup va SetPass
```

## Related notes
- [[Draw Call]]
- [[Optimization]]
- [[Batching]]
- [[GPU Instancing]]
- [[SRP Batcher]]
- [[Comparison]]

