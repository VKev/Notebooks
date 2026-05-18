---
aliases:
  - Visual Element
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `VisualElement` là building block cơ bản của UI Toolkit, mọi UI control đều kế thừa từ nó và tổ chức theo tree hierarchy.

## What is it
- Theo tài liệu Unity `6.3`, `VisualElement` là node cơ bản trong visual tree, tương tự `div` trong HTML, có thể chứa child element và nhận style.
- Mọi UI control như `Button`, `Label`, `TextField`, `Toggle`, `ScrollView`, `ListView`, `Slider`, `DropdownField` đều kế thừa từ `VisualElement`.
- Visual tree là đồ thị phân cấp gồm các `VisualElement`, root element được gọi là `rootVisualElement` và truy cập qua `UIDocument`.

## How it works
- Mỗi `VisualElement` có property `style` để set inline style, `name` để query, và `classList` để gắn USS class.
- Layout dùng hệ thống flexbox giống CSS: `flex-direction`, `justify-content`, `align-items`, `flex-grow`, `flex-shrink`.
- Event system dùng `RegisterCallback<TEvent>()` để lắng nghe event như `ClickEvent`, `ChangeEvent<T>`, `MouseEnterEvent`.
- Query element con bằng `Q<T>(name)` cho một element hoặc `Query<T>(className)` cho danh sách element.
- Có thể tạo element bằng code C# hoặc khai báo trong UXML, cả hai cách đều cho kết quả tương đương.

## Why use it
- Mô hình giống web development, dễ tiếp cận cho developer có kinh nghiệm HTML/CSS.
- Flexbox layout mạnh hơn và linh hoạt hơn hệ thống anchor/pivot của UGUI.
- Event system nhất quán cho cả Editor UI và runtime UI.

## When to use it
- Dùng cho mọi UI element trong UI Toolkit, từ container layout đến control phức tạp.
- Dùng khi cần tạo UI element custom bằng cách kế thừa `VisualElement`.
- Dùng query system để truy cập element từ code thay vì serialize reference.

## When to not use it
- Không dùng nếu project vẫn dùng UGUI và không có kế hoạch migrate.
- Không dùng cho world-space UI phức tạp, vì UI Toolkit runtime world-space support còn hạn chế so với UGUI.

## Limitations
- UI Toolkit runtime vẫn đang phát triển, một số feature có trong UGUI chưa có trong UI Toolkit.
- World-space UI support hạn chế hơn UGUI Canvas world space.
- Không có built-in physics interaction như UGUI `GraphicRaycaster`.

---

## Example code
```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class SimpleUI : MonoBehaviour
{
    void OnEnable()
    {
        var root = GetComponent<UIDocument>().rootVisualElement;

        // Tạo container với flexbox layout
        var container = new VisualElement();
        container.style.flexDirection = FlexDirection.Column;
        container.style.alignItems = Align.Center;
        container.style.paddingTop = 20;

        // Tạo label
        var label = new Label("Hello UI Toolkit");
        label.style.fontSize = 24;

        // Tạo button với event
        var button = new Button(() => Debug.Log("Clicked!"));
        button.text = "Click Me";
        button.style.width = 200;
        button.style.height = 40;

        container.Add(label);
        container.Add(button);
        root.Add(container);

        // Query element theo name
        var found = root.Q<Button>();
        Debug.Log($"Found button: {found.text}");
    }
}
```

---

## Related notes
- [[Definition]]
- [[UXML and USS]]
- [[UIDocument]]
- [[Summary]]
