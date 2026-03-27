---
aliases:
  - DOTS ECS vs MonoBehaviour
  - ECS vs OOP
  - ECS vs GameObject
note_type: permanent
created: 2026-03-27
tags:
  - unity
color: var(--mk-color-base-40)
---

## One-line answer
- `DOTS ECS` mạnh khi bài toán có nhiều data giống nhau và cần batch/parallel processing, còn `GameObject + MonoBehaviour` mạnh khi authoring trực quan, behavior phong phú, và object count không quá lớn.

## Problem
- Nhiều người nghe "ECS nhanh hơn" rồi muốn rewrite mọi thứ sang DOTS.
- Nếu không phân biệt đúng bài toán, bạn có thể tăng complexity mà không thu được lợi ích tương xứng.

## Core idea
- Đây không phải trận chiến "cái nào tốt hơn tuyệt đối", mà là "cái nào hợp với data shape và gameplay shape của hệ thống đó".
- `MonoBehaviour` thiên về object-oriented authoring.
- `DOTS ECS` thiên về data-oriented layout và bulk processing.

## Mechanism
- So sánh ngắn theo tiêu chí thực tế:
  - `Authoring`: `GameObject`/`MonoBehaviour` trực quan hơn; ECS thường author qua baking
  - `Data layout`: OOP phân tán hơn; ECS gom data theo archetype/chunk
  - `Logic`: OOP gắn behavior lên object; ECS tách logic vào system
  - `Scaling`: ECS thường hợp hơn khi entity count rất lớn
  - `Parallelism`: ECS phối hợp tốt với `Job System` và `Burst`
  - `Complexity`: ECS có learning curve và workflow khó hơn

## When to use
- Dùng `GameObject + MonoBehaviour` khi:
  - entity count vừa phải
  - authoring-driven workflow quan trọng
  - mỗi object có behavior riêng nhiều hơn là behavior theo batch
- Dùng `DOTS ECS` khi:
  - có số lượng entity lớn
  - data shape đồng nhất
  - system mang tính simulation hoặc transform/update hàng loạt
- Dùng hybrid approach khi:
  - chỉ một phần game thật sự cần DOTS scale

## Benefits
- So sánh kiểu này giúp tránh rewrite cực đoan.
- Team dễ chọn đúng kiến trúc cho từng subsystem thay vì áp một pattern cho toàn project.

## Trade-offs / Limits
- ECS đổi lấy hiệu năng và scale bằng complexity lớn hơn.
- OOP workflow dễ bắt đầu hơn nhưng có thể scale kém hơn trong bài toán mass simulation.
- Hybrid architecture tốt hơn "all-in ECS" trong nhiều project thực tế.

## Common mistakes
- Chuyển toàn bộ UI, menu, hoặc gameplay bespoke sang ECS dù không có lợi.
- Đánh giá ECS chỉ bằng tốc độ micro-benchmark mà bỏ qua workflow và maintainability.
- Nghĩ rằng DOTS là thay thế tuyệt đối cho mọi Unity workflow truyền thống.

## Example
- Rule of thumb:

```csharp
Few unique objects + rich authoring -> MonoBehaviour
Many similar entities + batch processing -> DOTS ECS
```

## Related notes
- [[DOTS ECS]]
- [[Definition]]
- [[Entity]]
- [[Components]]
- [[Systems]]
- [[Archetypes and Chunks]]
- [[Baking]]
- [[Job System]]
