---
aliases:
  - Unity SerializeReference
  - Managed Reference
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `[SerializeReference]` yêu cầu Unity serialize một field dưới dạng managed reference thay vì serialize inline theo value.

## Key points
- Theo Unity `6.4 (6000.4)`, `[SerializeReference]` dùng cho custom class cần giữ identity, null, shared reference, cyclic graph, hoặc polymorphism.
- Mặc định, custom class không kế thừa `UnityEngine.Object` được Unity serialize inline theo value.
- Khi dùng `[SerializeReference]`, Unity lưu object được reference trong phần managed reference registry của host object.
- Host object là object chứa field `[SerializeReference]`, ví dụ `MonoBehaviour`, `ScriptableObject`, hoặc một Unity object khác.
- Mỗi managed reference có ID riêng trong phạm vi host object.
- Unity lưu type đầy đủ và field value của managed reference để có thể dựng lại đúng derived type khi deserialize.
- Managed reference không được share xuyên qua nhiều host object; nếu cần share data giữa nhiều object, dùng `ScriptableObject` asset.

## Decision rules
- Khi field cần chứa nhiều implementation khác nhau sau cùng một interface hoặc base class.
- Khi cần biểu diễn graph, linked list, behavior tree, hoặc data có reference vòng.
- Khi cần phân biệt `null` với object rỗng trong serialized data.
- Không dùng cho data phẳng, đơn giản, không cần polymorphism hoặc shared reference.
- Không dùng để reference `GameObject`, `MonoBehaviour`, `ScriptableObject`, `Transform`, hoặc type kế thừa `UnityEngine.Object`.
- Không dùng cho `struct` hoặc primitive type; các type đó nên serialize theo value.
- Giữ được `null` trong field custom class.
- Giữ được nhiều field cùng trỏ tới cùng một instance trong cùng host object.

## Example
```csharp
using System;
using UnityEngine;
public class AbilityHolder : MonoBehaviour
{
    [SerializeReference] private Ability ability;
}
[Serializable]
public abstract class Ability
{
    public string displayName;
    public abstract void Use();
}
[Serializable]
public class HealAbility : Ability
{
    public int amount;
    public override void Use()
    {
        Debug.Log($"Heal {amount}");
    }
}
```

## Related notes
- [[Definition]]
- [[SerializeField]]
- [[../Scriptable Object/Scriptable Object|Scriptable Object]]
- [[Summary]]
