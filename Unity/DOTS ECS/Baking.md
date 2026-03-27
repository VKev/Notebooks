---
aliases:
  - ECS Baking
  - Baker
  - Authoring to Runtime
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Baking` là quá trình Unity đổi `GameObject` authoring data trong Editor thành ECS runtime data tối ưu cho hiệu năng; quá trình này chỉ xảy ra trong Editor, không xảy ra trong game runtime.

## Problem
- GameObject authoring linh hoạt và dễ chỉnh sửa, nhưng không tối ưu cho runtime ECS processing.
- Bạn cần một cầu nối giữa editor-friendly data và runtime-friendly data.

## Core idea
- Authoring data dành cho con người chỉnh sửa.
- Runtime ECS data dành cho máy xử lý nhanh.
- Baking là bước chuyển đổi giữa hai miền đó.

## Mechanism
- Baking docs mô tả:
  - baking chuyển authoring GameObject data thành entity data
  - baking chỉ chạy trong Editor
  - baking có thể là full bake hoặc incremental/live bake
- `Baker` là nơi bạn định nghĩa cách đổi authoring component sang ECS component.
- Subscene và baking workflow quyết định khi nào ECS data được sinh lại.

## When to use
- Dùng khi project vẫn author bằng `GameObject` hoặc scene workflow của Unity nhưng runtime chạy bằng ECS.
- Hợp cho hybrid workflow giữa authoring truyền thống và ECS runtime.

## Benefits
- Giữ authoring workflow dễ dùng hơn.
- Tạo runtime data tối ưu hơn cho ECS.
- Cho phép live/incremental update trong Editor với subscene workflow.

## Trade-offs / Limits
- Baking tăng thêm một layer workflow và mental model.
- Full bake và incremental bake có khác biệt về chunk layout, nên baking code phải nhất quán.
- Không nên nhầm baking với runtime conversion xảy ra trong Play mode.

## Common mistakes
- Nghĩ rằng baking xảy ra liên tục trong build runtime.
- Dùng authoring assumptions như thể runtime ECS data giống y hệt GameObject data.
- Viết baking code mà kết quả incremental bake và full bake không nhất quán.

## Example
- Baker thêm runtime ECS component từ authoring MonoBehaviour:

```csharp
class Baker : Baker<EnemyAuthoring>
{
    public override void Bake(EnemyAuthoring authoring)
    {
        var entity = GetEntity(TransformUsageFlags.Dynamic);
        AddComponent(entity, new Speed { Value = authoring.Speed });
    }
}
```

## Related notes
- [[DOTS ECS]]
- [[Definition]]
- [[Components]]
- [[World]]
- [[Job System]]
