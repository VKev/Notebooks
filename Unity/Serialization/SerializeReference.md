---
aliases:
  - Unity SerializeReference
  - Managed Reference
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `[SerializeReference]` yêu cầu Unity serialize một field dưới dạng managed reference thay vì serialize inline theo value.

## What is it
- Theo Unity `6.3 LTS (6000.3)`, `[SerializeReference]` dùng cho custom class cần giữ identity, null, shared reference, cyclic graph, hoặc polymorphism.
- Mặc định, custom class không kế thừa `UnityEngine.Object` thường được Unity serialize inline theo value.
- Khi dùng `[SerializeReference]`, Unity lưu object được reference trong phần managed reference registry của host object.

## How it works
- Host object là object chứa field `[SerializeReference]`, ví dụ `MonoBehaviour`, `ScriptableObject`, hoặc một Unity object khác.
- Mỗi managed reference có ID riêng trong phạm vi host object.
- Unity lưu type đầy đủ và field value của managed reference để có thể dựng lại đúng derived type khi deserialize.
- Managed reference không được share xuyên qua nhiều host object; nếu cần share data giữa nhiều object, dùng `ScriptableObject` asset.

## Why use it
- Giữ được `null` trong field custom class.
- Giữ được nhiều field cùng trỏ tới cùng một instance trong cùng host object.
- Hỗ trợ polymorphism khi field khai báo interface hoặc base class nhưng runtime object là derived class.
- Hỗ trợ graph hoặc cycle như tree node, behavior graph, dialogue graph.

## When to use it
- Dùng khi field cần chứa nhiều implementation khác nhau sau cùng một interface hoặc base class.
- Dùng khi cần biểu diễn graph, linked list, behavior tree, hoặc data có reference vòng.
- Dùng khi cần phân biệt `null` với object rỗng trong serialized data.

## When to not use it
- Không dùng cho data phẳng, đơn giản, không cần polymorphism hoặc shared reference.
- Không dùng để reference `GameObject`, `MonoBehaviour`, `ScriptableObject`, `Transform`, hoặc type kế thừa `UnityEngine.Object`.
- Không dùng cho `struct` hoặc primitive type; các type đó nên serialize theo value.

## Limitations
- Tốn thêm storage, memory, và thời gian load/save hơn inline serialization.
- Reference chỉ ổn định trong phạm vi host object, không phải asset reference toàn project.
- Inspector mặc định có thể cần custom drawer hoặc tooling để chọn derived type dễ dùng hơn.

---

## Example code
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

---

## Related notes
- [[Definition]]
- [[SerializeField]]
- [[../Scriptable Object/Scriptable Object|Scriptable Object]]
- [[Summary]]
