---
aliases:
  - UI Document Component
  - PanelSettings
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `UIDocument` là component gắn lên `GameObject` để hiển thị UI Toolkit tại runtime, kết nối UXML template với scene thông qua `PanelSettings`.

## Key points
- Unity `6.3`: `UIDocument` là cầu nối giữa UI Toolkit và scene, nó load `VisualTreeAsset` (file UXML) và hiển thị UI trên screen.
- `PanelSettings` là `ScriptableObject` cấu hình cách panel render: scale mode, sort order, reference resolution, và theme USS.
- `rootVisualElement` trên `UIDocument` là entry point để code C# truy cập và thao tác với UI element.
- Tạo `GameObject` trong scene, thêm component `UIDocument`.
- Gán `VisualTreeAsset` (UXML file) vào field `Source Asset` để định nghĩa cấu trúc UI.
- Gán `PanelSettings` asset để cấu hình rendering: scale mode (`Constant Pixel Size`, `Scale With Screen Size`, `Constant Physical Size`), reference resolution, và sort order.
- Khi vào Play Mode, `UIDocument` tự động load UXML và tạo visual tree.
- Code C# truy cập `rootVisualElement` từ `UIDocument` component để query element, đăng ký event, và update data.
- Nhiều `UIDocument` có thể cùng tồn tại trong scene, `sort order` quyết định thứ tự vẽ.

## Decision rules
- Là cách chính thức để hiển thị UI Toolkit tại runtime, thay thế `Canvas` trong UGUI.
- `PanelSettings` cho phép quản lý scale và theme tập trung cho nhiều UI panel.
- `rootVisualElement` cung cấp API mạnh để query và thao tác UI từ code.
- Dùng cho mỗi màn hình UI hoặc panel UI cần hiển thị trong game: HUD, menu, inventory, dialog.
- Dùng `PanelSettings` chia sẻ giữa nhiều `UIDocument` để giữ scale và theme nhất quán.
- Không cần cho Editor-only UI, dùng `EditorWindow` trực tiếp.
- Tránh nếu project vẫn dùng UGUI Canvas và không migrate.
- Mỗi `UIDocument` cần một `PanelSettings` asset, thiếu sẽ không render.
- World-space UI support hạn chế hơn so với UGUI world-space Canvas.
- Interaction giữa UI Toolkit panel và UGUI Canvas cần xử lý event routing thủ công.

## Example
```csharp
using UnityEngine;
using UnityEngine.UIElements;
public class GameHUD : MonoBehaviour
{
    private Label _scoreLabel;
    private Button _pauseButton;
    private ProgressBar _healthBar;
    void OnEnable()
    {
        var root = GetComponent<UIDocument>().rootVisualElement;
        _scoreLabel = root.Q<Label>("score-label");
        _pauseButton = root.Q<Button>("pause-button");
        _healthBar = root.Q<ProgressBar>("health-bar");
        _pauseButton.RegisterCallback<ClickEvent>(evt =>
        {
            Time.timeScale = Time.timeScale == 0 ? 1 : 0;
            _pauseButton.text = Time.timeScale == 0 ? "Resume" : "Pause";
        });
    }
    public void UpdateScore(int score)
    {
        _scoreLabel.text = $"Score: {score}";
    }
    public void UpdateHealth(float current, float max)
    {
        _healthBar.value = current;
        _healthBar.highValue = max;
    }
}
```

## Related notes
- [[Definition]]
- [[VisualElement]]
- [[UXML and USS]]
- [[Summary]]
