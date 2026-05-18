---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.3 LTS (6000.3)`: version tài liệu chính dùng để kiểm tra serialization rules, `[SerializeField]`, và `[SerializeReference]` trong section này.

## Core keywords
- `Serialization`: Quá trình Unity lưu state thành dạng có thể dựng lại sau. Ảnh hưởng trực tiếp tới Inspector, scene, prefab, asset, và reload behavior.
- `Field serialization`: Unity serializer làm việc trên field, không phải property theo mặc định. Field phải public hoặc có `[SerializeField]`, không static, không const, không readonly, và type phải được Unity hỗ trợ.
- `[SerializeField]`: Cho phép private field được Unity serialize. Dùng để giữ encapsulation nhưng vẫn expose config trong Inspector.
- `[SerializeReference]`: Serialize custom class dưới dạng managed reference thay vì inline value. Dùng khi cần null, polymorphism, shared reference, hoặc graph/cycle.

## Serializable field types
- `Primitive and Unity built-in types`: Ví dụ `int`, `float`, `bool`, `string`, `Vector3`, `Color`, `AnimationCurve`. Đây là nhóm được Unity serialize phổ biến nhất.
- `UnityEngine.Object reference`: Reference tới object kế thừa `UnityEngine.Object` được serialize như reference. Ví dụ `GameObject`, `Transform`, `Texture`, `ScriptableObject`.
- `Custom class or struct`: Cần `[Serializable]` nếu không kế thừa `UnityEngine.Object`. Mặc định custom class được serialize inline theo value, không giữ identity.
- `Array and List`: Unity hỗ trợ array hoặc `List<T>` nếu `T` là type serialize được. Không hỗ trợ trực tiếp multidimensional array, jagged array, dictionary, hoặc nested container phức tạp.

## Decision rules
- `Cần chỉnh private config trong Inspector`: Dùng `[SerializeField] private`. Runtime code đọc qua property hoặc method, không ghi trực tiếp field.
- `Cần polymorphism cho serialized custom class`: Dùng `[SerializeReference]` trên field base class hoặc interface. Derived class vẫn cần `[Serializable]`.
- `Cần share data giữa nhiều object hoặc nhiều scene`: Dùng `ScriptableObject`, không dùng `[SerializeReference]`. Managed reference chỉ nằm trong phạm vi host object.
- `Cần serialize dictionary hoặc nested container`: Wrap data trong class/struct serialize được hoặc dùng `ISerializationCallbackReceiver`. Không mong Unity serialize dictionary trực tiếp.

## Common traps
- `Public field luôn là thiết kế tốt vì Inspector cần thấy`: Sai. Dùng `[SerializeField] private` để tách Inspector exposure khỏi public API.
- `Unity serialize property giống C# serializer`: Sai. Unity serialize field; property chỉ được serialize nếu backing field được gắn attribute đúng cách.
- `Custom class reference sẽ giữ shared identity mặc định`: Sai. Inline serialization tạo copy riêng khi serialize nhiều field trỏ cùng một instance.
- `[SerializeReference] là default tốt hơn`: Sai. Nó có overhead cao hơn và chỉ nên dùng khi cần null, polymorphism, shared reference, hoặc graph.
- `Dictionary serialize trực tiếp trong Inspector`: Sai. Unity không hỗ trợ trực tiếp dictionary trong serialization rules mặc định.

## Review questions

### Unity serialization khác .NET serialization ở điểm nào?
- Unity dùng serialization system riêng, tối ưu cho runtime và Editor của Unity. Nó làm việc theo rule riêng về field, type được hỗ trợ, Inspector, scene, prefab, và asset.

### Khi nào dùng `[SerializeField] private` thay vì public field?
- Khi field cần chỉnh trong Inspector nhưng không nên trở thành public API cho code ngoài class. Đây là cách giữ encapsulation tốt hơn.

### Unity có serialize property không?
- Không theo mặc định. Unity serialize field. Với auto-property, có thể dùng `[field: SerializeField]` để serialize backing field.

### Vì sao dictionary không hiện hoặc không serialize đúng trong Inspector?
- Unity serialization rules không hỗ trợ dictionary trực tiếp. Cần wrap data, chuyển thành list pair, hoặc dùng custom serialization callback.

### `[SerializeReference]` giải quyết vấn đề gì?
- Nó giữ managed reference cho custom class, hỗ trợ null, shared reference, graph/cycle, và polymorphism mà inline serialization không giữ được.

### Khi nào không nên dùng `[SerializeReference]`?
- Khi data đơn giản, phẳng, không cần identity hoặc polymorphism. Inline serialization hiệu quả hơn về storage, memory, và load/save time.

## Related notes
- [[Serialization]]
- [[Definition]]
- [[SerializeField]]
- [[SerializeReference]]
- [[../Scriptable Object/Scriptable Object|Scriptable Object]]
- [[../MonoBehaviour Lifecycle/MonoBehaviour Lifecycle|MonoBehaviour Lifecycle]]
- [[../Unity|Unity]]

