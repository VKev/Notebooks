---
aliases:
  - Unity SerializeField
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `[SerializeField]` buộc Unity serialize một private field để field đó vẫn được lưu và hiển thị trong Inspector.

## Key points
- Theo Unity `6.3 LTS (6000.3)`, Unity serialize public field theo mặc định, còn private field cần thêm `[SerializeField]`.
- Attribute này giúp giữ encapsulation ở mức C# bằng `private`, nhưng vẫn cho designer chỉnh giá trị trong Inspector.
- Unity serializer làm việc trực tiếp trên field, không serialize property theo cách thông thường.
- Unity serialize field nếu field đó là `public` hoặc có `[SerializeField]`.
- Field vẫn phải thỏa rule còn lại: không phải `static`, không phải `const`, không phải `readonly`, và type phải nằm trong nhóm Unity serialize được.
- Với auto-property, có thể dùng cú pháp `[field: SerializeField]` để attribute gắn vào backing field do compiler tạo ra.
- Khi script reload, Unity lưu field đã serialize, reload assembly, rồi dựng lại component từ dữ liệu đã serialize.

## Decision rules
- Giữ data quan trọng không bị public API hóa chỉ để hiện trong Inspector.
- Giúp code rõ ownership hơn: code ngoài class không tùy tiện ghi field, nhưng Unity Editor vẫn chỉnh được giá trị cấu hình.
- Rất dùng cho config của `MonoBehaviour` và `ScriptableObject`.
- Dùng cho field cần chỉnh trong Inspector nhưng không nên bị code ngoài class thay đổi trực tiếp.
- Khi muốn expose giá trị cho designer mà vẫn cung cấp read-only property cho runtime code.
- Không dùng cho data runtime tạm thời không cần persist qua reload hoặc không cần hiển thị trong Inspector.
- Không dùng để serialize type Unity không hỗ trợ như dictionary trực tiếp, multidimensional array, hoặc nested container phức tạp.
- `[SerializeField]` không làm Unity serialize được mọi type; field vẫn phải tuân theo serialization rules.
- Không áp dụng cho `static`, `const`, hoặc `readonly` field.
- Attribute này không thay thế validation; nếu Inspector có thể nhập giá trị sai, vẫn nên validate bằng `OnValidate` hoặc setter riêng.

## Example
```csharp
using UnityEngine;
public class PlayerHealth : MonoBehaviour
{
    [SerializeField] private int maxHealth = 100;
    public int MaxHealth => maxHealth;
    private void OnValidate()
    {
        maxHealth = Mathf.Max(1, maxHealth);
    }
}
```

## Related notes
- [[Definition]]
- [[SerializeReference]]
- [[../MonoBehaviour Lifecycle/MonoBehaviour Lifecycle|MonoBehaviour Lifecycle]]
- [[../Scriptable Object/Scriptable Object|Scriptable Object]]
- [[Summary]]
