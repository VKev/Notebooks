---
aliases:
  - DOTS/ECS
  - Unity DOTS
  - Unity ECS
  - ECS
note_type: permanent
created: 2026-03-27
tags:
  - unity
color: var(--mk-color-base-40)
---

## One-line answer
- `DOTS ECS` là model data-oriented của Unity, trong đó `Entity` là ID nhẹ, `Component` là data thuần, `System` là logic xử lý data; DOTS rộng hơn ECS và thường đi cùng `Job System`, `Burst`, và collections tối ưu cho hiệu năng.

## Problem
- Với `GameObject` và `MonoBehaviour`, data và behavior thường bị gắn theo object, dễ dẫn tới layout memory phân tán và overhead cao khi số lượng object lớn.
- Những bài toán như `crowd`, `boids`, `projectile swarm`, hoặc simulation hàng chục nghìn thực thể cần cách tổ chức data hiệu quả hơn.

## Core idea
- ECS tách `identity`, `data`, và `logic` thành ba phần riêng:
  - `Entity`: ID
  - `Component`: data
  - `System`: code xử lý data
- DOTS là umbrella lớn hơn, trong đó `Entities` package cung cấp ECS, còn `Job System`, `Burst`, và native collections giúp scale compute tốt hơn.

## Mechanism
- Unity Entities docs mô tả ECS là kiến trúc tổ chức code và data theo kiểu data-oriented.
- Ở runtime, entity được group theo `archetype` và lưu trong `chunk` để batch processing hiệu quả hơn.
- Ở authoring time, Unity dùng `baking` để đổi `GameObject` data sang ECS runtime data.
- ECS đặc biệt mạnh khi đi cùng:
  - `Job System` cho parallel compute
  - `Burst` cho optimized native code
  - `NativeContainer` cho memory layout và data access phù hợp hơn

## When to use
- Hợp khi có số lượng entity lớn và logic lặp lại theo pattern rõ ràng.
- Hợp cho data-oriented gameplay, simulation, AI crowd, physics-like processing, hoặc systems CPU-bound có thể chạy batch.
- Không phải lựa chọn bắt buộc cho mọi game system nhỏ, UI-heavy flow, hoặc authored behavior độc nhất.

## Benefits
- Data layout cache-friendly hơn.
- Logic dễ batch và scale tốt hơn khi entity count cao.
- Kết hợp tốt với `Burst` và `Job System`.

## Trade-offs / Limits
- Mental model khó hơn `MonoBehaviour`.
- Authoring workflow phức tạp hơn vì phải qua baking và runtime data model riêng.
- Không phải cứ chuyển sang ECS là game tự nhanh; phải đúng bài toán và đúng layout data.

## Common mistakes
- Nghĩ rằng DOTS chỉ là "nhiều thread hơn".
- Dùng ECS cho hệ thống ít object nhưng behavior cực riêng lẻ, khiến complexity tăng mà lợi ích ít.
- Lẫn lộn giữa `Entities` package, `Job System`, và toàn bộ khái niệm DOTS.

## Example
- Mental map ngắn của DOTS/ECS:

```csharp
Entity   // identity
Component // pure data
System   // logic over matching data
```

## Related notes
- [[DOTS ECS]]
- [[Comparison]]
- [[Entity]]
- [[Components]]
- [[Systems]]
- [[World]]
- [[Archetypes and Chunks]]
- [[Baking]]
- [[Job System]]
