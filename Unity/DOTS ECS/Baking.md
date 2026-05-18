---
aliases:
  - ECS Baking
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Baking` là quá trình chuyển dữ liệu GameObject authoring trong Editor thành entity và component runtime cho ECS.

## Key points
- Theo `Entities 1.4.2`, baking biến authoring data từ GameObject thành runtime data ghi vào Entity Scenes.
- quá trình không đảo ngược, nhằm đổi một mô hình authoring linh hoạt nhưng nặng sang mô hình runtime tối ưu cho ECS.
- Baking chỉ chạy trong Editor, không chạy in-game.
- Khi authoring data thay đổi, Unity sẽ bake lại tùy theo trạng thái subscene.
- Nếu subscene đang mở, Unity có thể live bake theo kiểu incremental hoặc full bake.
- Nếu subscene đóng, Unity bake bất đồng bộ ở background importer và thực hiện full bake.

## Decision rules
- Giúp giữ workflow authoring quen thuộc bằng `GameObject`/`MonoBehaviour` nhưng runtime vẫn dùng ECS tối ưu.
- Là cầu nối quan trọng giữa authoring world và runtime ECS world.
- Khi bạn xây entity từ prefab, scene, hoặc authoring component trong workflow ECS.
- Khi muốn dữ liệu runtime được sinh ra trước và tối ưu hơn lúc vào Play mode.
- Không cần nghĩ sâu về baking nếu project chưa dùng Entities/SubScene.
- Tránh đặt giả định rằng baking chạy lúc runtime như một bước bình của gameplay.
- Baking chỉ có trong Editor và có thêm độ phức tạp về pipeline.
- Incremental bake và full bake có thể khác nhau ở entity ordering và chunk layout, nên code baking phải nhất quán.

## Example
```csharp
using Unity.Entities;
using UnityEngine;
public class EnemyAuthoring : MonoBehaviour
{
    public float Speed = 5f;
    class Baker : Baker<EnemyAuthoring>
    {
        public override void Bake(EnemyAuthoring authoring)
        {
            var entity = GetEntity(TransformUsageFlags.Dynamic);
            AddComponent(entity, new Speed { Value = authoring.Speed });
        }
    }
}
public struct Speed : IComponentData
{
    public float Value;
}
```

## Related notes
- [[Definition]]
- [[Components]]
- [[World]]
- [[Summary]]
