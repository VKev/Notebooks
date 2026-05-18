---
aliases:
  - UXML
  - USS
  - UI Markup and Style
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `UXML` định nghĩa cấu trúc UI bằng markup giống XML, còn `USS` định nghĩa style bằng cú pháp giống CSS, tách biệt structure và presentation.

## Key points
- Unity `6.3`: `UXML` (Unity eXtensible Markup Language) là format khai báo cấu trúc UI element và template tái sử dụng, lấy cảm hứng từ HTML/XML.
- `USS` (Unity Style Sheets) là format khai báo visual style cho element, lấy cảm hứng từ CSS, hỗ trợ selector, class, pseudo-class, và variable.
- Tách biệt UXML và USS giúp artist chỉnh style mà không ảnh hưởng logic, và programmer chỉnh logic mà không ảnh hưởng layout.
- UXML file chứa hierarchy của element với namespace `UnityEngine.UIElements`, ví dụ `<ui:Button text="Play" />`.
- USS file chứa rule gồm selector và property, ví dụ `.button-primary { background-color: blue; font-size: 16px; }`.
- UXML có thể reference USS file bằng `<Style src="styles.uss" />` hoặc gán USS qua code.
- USS selector hỗ trợ: type selector (`Button`), class selector (`.my-class`), name selector (`#my-name`), pseudo-class (`:hover`, `:active`, `:focus`), và descendant selector.
- USS hỗ trợ variable: `--my-color: blue;` và dùng `var(--my-color)`.
- `UI Builder` là visual editor cho phép kéo thả để tạo UXML và USS mà không cần viết text.

## Decision rules
- Tách biệt concern: structure (UXML), style (USS), và logic (C#) giống mô hình HTML/CSS/JS.
- USS cho phép restyle toàn bộ UI bằng cách đổi file USS mà không sửa UXML hay code.
- Template UXML có thể tái sử dụng qua `<ui:Template>` và `<ui:Instance>`.
- UI Builder giúp artist thiết kế UI trực quan mà không cần viết markup.
- Dùng UXML cho UI có layout cố định hoặc template tái sử dụng.
- Dùng USS khi cần style nhất quán cho nhiều element hoặc theme switching.
- Dùng UI Builder khi muốn thiết kế UI bằng drag-and-drop.
- Không cần UXML nếu UI hoàn toàn dynamic và tạo bằng code C# dựa trên data runtime.
- Không cần USS cho prototype nhanh với vài element, inline style trên code đủ dùng.
- USS không hỗ trợ tất cả CSS property, chỉ một subset phù hợp với Unity.
- UXML không hỗ trợ conditional rendering hay loop, logic phải xử lý bằng C#.
- UI Builder đôi khi tạo UXML verbose hơn viết tay.

## Example
```xml
<!-- GameUI.uxml -->
<ui:UXML xmlns:ui="UnityEngine.UIElements">
    <Style src="GameUI.uss" />
    <ui:VisualElement class="container">
        <ui:Label text="Score: 0" name="score-label" class="title" />
        <ui:Button text="Start Game" name="start-button" class="btn-primary" />
        <ui:ProgressBar name="health-bar" title="HP" high-value="100" />
    </ui:VisualElement>
</ui:UXML>
```

```css
/* GameUI.uss */
.container {
    flex-direction: column;
    align-items: center;
    padding: 20px;
}
.title {
    font-size: 32px;
    color: white;
    -unity-font-style: bold;
}
.btn-primary {
    width: 200px;
    height: 50px;
    background-color: rgb(50, 120, 200);
    border-radius: 8px;
    color: white;
    font-size: 18px;
}
.btn-primary:hover {
    background-color: rgb(70, 140, 220);
}
```

## Related notes
- [[Definition]]
- [[VisualElement]]
- [[UIDocument]]
- [[Summary]]
