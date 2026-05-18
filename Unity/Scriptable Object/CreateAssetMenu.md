---
aliases:
  - CreateAssetMenu Attribute
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `CreateAssetMenu` là attribute cho phép tạo instance của `ScriptableObject` trực tiếp từ menu Create trong Editor.

## Key points
- Unity `6.3 LTS (6000.3)`: `[CreateAssetMenu]` là attribute gắn trên class kế thừa `ScriptableObject` để tự động đăng ký type đó vào Assets > Create submenu.
- Nó có ba property tùy chọn: `menuName` để đặt tên và vị trí hiển thị trong menu, `fileName` để đặt tên mặc định cho asset mới, và `order` để xác định thứ tự trong menu.
- Theo docs Unity, `fileName` tùy chỉnh nên kết thúc bằng `.asset` để Unity xử lý đúng asset mới tạo.
- Khi attribute này được thêm, bạn có thể right-click trong Project window và tạo asset `.asset` mà không cần viết thêm code Editor nào.
- Unity đọc attribute `[CreateAssetMenu]` từ class lúc compile và đăng ký vào context menu của Project window.
- Khi bạn chọn menu item tương ứng, Unity tạo instance của type được đánh dấu, serialize thành file `.asset`, và lưu vào thư mục đang chọn trong Project.
- Tham số `menuName` hỗ trợ nested path bằng dấu `/`, ví dụ `"Game/Item Data"` sẽ tạo submenu `Game` rồi mục `Item Data` bên trong.
- Unity loại bỏ trailing space ở cuối `menuName`, nên hai menu name chỉ khác nhau bởi dấu cách cuối sẽ được xem như cùng một menu item.

## Decision rules
- Giúp designer và artist tạo data asset trực tiếp trong Editor mà không cần viết code.
- Giảm rào cản khi cần thêm nhiều instance data khác nhau, ví dụ nhiều loại item, nhiều bộ stats, nhiều level config.
- Khi bạn muốn cung cấp workflow tạo data asset nhanh trong Editor cho cả team.
- Khi class `ScriptableObject` dự kiến sẽ có nhiều instance khác nhau, ví dụ item database, enemy config, level setting.
- Bỏ qua nếu class `ScriptableObject` chỉ có duy nhất một instance cố định và được tạo thủ công một lần.
- Bỏ qua nếu bạn chỉ tạo instance bằng code lúc runtime thông qua `CreateInstance<T>()`.
- `[CreateAssetMenu]` chỉ hoạt động trong Editor, không có tác dụng trong build.
- Attribute này chỉ áp dụng cho type kế thừa `ScriptableObject`, không dùng để tạo `MonoBehaviour` hay `GameObject`.
- Tham số `order` có phạm vi ảnh hưởng hạn chế và thứ tự menu có thể không chính xác tuyệt đối khi có nhiều attribute cùng đăng ký.

## Example
```csharp
using UnityEngine;
[CreateAssetMenu(fileName = "NewItemData.asset", menuName = "Game/Item Data", order = 1)]
public class ItemData : ScriptableObject
{
    public string itemName;
    public Sprite icon;
    public int maxStack = 99;
    public float weight;
}
```

## Related notes
- [[Definition]]
- [[Lifecycle]]
- [[Data Persistence]]
- [[Summary]]
