---
aliases:
  - ECS World
  - World trong ECS
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `World` là container runtime của ECS, nơi chứa entities, systems, và state liên quan tới một execution context cụ thể.

## Problem
- Nếu chỉ nghĩ tới entity và component mà quên runtime context, bạn sẽ khó hiểu vì sao system này chạy ở đâu, thuộc group nào, hay vì sao có nhiều thế giới song song.

## Core idea
- World là "bối cảnh runtime" của ECS.
- Entity và system không tồn tại trong chân không; chúng sống trong một world cụ thể.

## Mechanism
- World concepts trong Entities docs giải thích world như container cho entities và systems.
- Một project có thể có nhiều world cho các mục đích khác nhau như runtime, conversion/baking, hoặc testing.
- System update order và grouping cũng gắn với world tương ứng.

## When to use
- Dùng note này để hiểu lifecycle và execution context của ECS.
- Quan trọng khi debug system order, bootstrap, hoặc custom world setup.

## Benefits
- Giúp bạn hiểu ECS runtime ở mức tổ chức tổng thể.
- Là nền để hiểu system groups và world-specific execution.

## Trade-offs / Limits
- Đây là concept trừu tượng hơn entity/component, nên lúc đầu dễ bị bỏ qua.
- Với project đơn giản, bạn có thể không chạm sâu vào world customization ngay.

## Common mistakes
- Nghĩ rằng toàn bộ ECS runtime chỉ có một world duy nhất.
- Không để ý system đang thuộc world nào khi debug.
- Nhầm baking world với gameplay runtime world.

## Example
- Mental model:

```csharp
World = container of entities + systems + execution context
```

## Related notes
- [[DOTS ECS]]
- [[Definition]]
- [[Systems]]
- [[Baking]]
