---
aliases:
  - ScriptableObject Event
  - SO Event Channel
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Event Channel` là pattern dùng `ScriptableObject` làm kênh trung gian để broadcast event giữa các hệ thống mà không cần chúng biết nhau.

## What is it
- Event Channel là một `ScriptableObject` asset đóng vai trò trung gian: hệ thống muốn gửi event thì gọi `Raise()` trên asset, hệ thống muốn nhận thì đăng ký listener trên cùng asset đó.
- Pattern này phổ biến từ bài talk của Ryan Hipple tại Unite 2017 và được Unity chính thức hướng dẫn trong tài liệu kiến trúc `ScriptableObject`.
- So với static event hoặc singleton, Event Channel có thể kéo thả trong Inspector, dễ test và debug hơn vì có thể nhìn thấy asset trong project.

## How it works
- Tạo một class `ScriptableObject` đóng vai trò event, bên trong giữ danh sách listener thường là `System.Action` hoặc `UnityAction`.
- Hệ thống publisher gọi method `Raise()` trên asset để phát event.
- `Raise()` duyệt qua danh sách listener và gọi từng callback.
- Hệ thống subscriber đăng ký trong `OnEnable` và hủy đăng ký trong `OnDisable` bằng cách reference cùng asset đó.
- Vì event channel là asset, nó tồn tại ngoài scene và không bị ảnh hưởng khi chuyển scene.

## Why use it
- Decoupling hoàn toàn giữa publisher và subscriber, không cần reference trực tiếp tới `GameObject` hay `MonoBehaviour` cụ thể.
- Designer có thể kéo thả event asset vào Inspector để nối hệ thống mà không cần chỉnh code.
- Dễ test từng hệ thống riêng lẻ bằng cách tạo event asset giả hoặc gọi `Raise()` từ Editor script.

## When to use it
- Dùng khi nhiều hệ thống cần phản ứng với cùng một sự kiện, ví dụ player chết, level hoàn thành, item nhặt được.
- Dùng khi muốn nối event giữa các scene khác nhau mà không phụ thuộc vào scene hierarchy.
- Dùng khi team có designer cần tự cấu hình event flow mà không cần lập trình viên hỗ trợ.

## When to not use it
- Không phù hợp cho event chỉ xảy ra cục bộ trong một component, dùng C# event hoặc delegate thông thường sẽ đơn giản hơn.
- Không nên dùng cho event tần suất rất cao mỗi frame, vì delegate invocation qua list có overhead cao hơn direct call.

## Limitations
- Listener cần đăng ký và hủy đăng ký đúng cách trong `OnEnable`/`OnDisable`, nếu quên hủy sẽ gây null reference hoặc memory leak.
- Debug flow khó hơn khi số lượng event channel lớn và kết nối phức tạp giữa nhiều hệ thống.
- Không hỗ trợ return value, nếu cần response phải dùng pattern phức tạp hơn hoặc callback riêng.

---

## Example code
```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

[CreateAssetMenu(fileName = "NewGameEvent.asset", menuName = "Game/Event Channel")]
public class GameEvent : ScriptableObject
{
    private readonly List<UnityAction> listeners = new List<UnityAction>();

    public void Raise()
    {
        for (int i = listeners.Count - 1; i >= 0; i--)
            listeners[i]?.Invoke();
    }

    public void Register(UnityAction action) => listeners.Add(action);
    public void Unregister(UnityAction action) => listeners.Remove(action);
}

// Publisher: phát event khi player chết
public class PlayerHealth : MonoBehaviour
{
    public GameEvent onPlayerDied;
    private float health = 100f;

    public void TakeDamage(float amount)
    {
        health -= amount;
        if (health <= 0f)
            onPlayerDied.Raise();
    }
}

// Subscriber: lắng nghe event và hiển thị UI
public class UIGameOver : MonoBehaviour
{
    public GameEvent onPlayerDied;
    public GameObject gameOverPanel;

    void OnEnable() => onPlayerDied.Register(ShowGameOver);
    void OnDisable() => onPlayerDied.Unregister(ShowGameOver);

    void ShowGameOver() => gameOverPanel.SetActive(true);
}
```

---

## Related notes
- [[Definition]]
- [[Runtime Set]]
- [[Data Persistence]]
- [[Summary]]
