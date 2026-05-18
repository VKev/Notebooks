---
aliases:
  - ScriptableObject Event
  - SO Event Channel
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Event Channel` là pattern dùng `ScriptableObject` làm kênh trung gian để broadcast event giữa các hệ thống mà không cần chúng biết nhau.

## Key points
- Event Channel là `ScriptableObject` asset đóng vai trò trung gian: hệ thống muốn gửi event thì gọi `Raise()` trên asset, hệ thống muốn nhận thì đăng ký listener trên cùng asset đó.
- Pattern này phổ biến từ bài talk của Ryan Hipple tại Unite 2017 và được Unity chính thức hướng dẫn trong tài liệu kiến trúc `ScriptableObject`.
- So với static event hoặc singleton, Event Channel có thể kéo thả trong Inspector, dễ test và debug hơn vì có thể nhìn thấy asset trong project.
- Tạo một class `ScriptableObject` đóng vai trò event, bên trong giữ danh sách listener là `System.Action` hoặc `UnityAction`.
- Hệ thống publisher gọi method `Raise()` trên asset để phát event.
- `Raise()` duyệt qua danh sách listener và gọi từng callback.
- Hệ thống subscriber đăng ký trong `OnEnable` và hủy đăng ký trong `OnDisable` bằng cách reference cùng asset đó.
- vì event channel là asset, tồn tại ngoài scene và không bị ảnh hưởng khi chuyển scene.

## Decision rules
- Khi nhiều hệ thống cần phản ứng với cùng một sự kiện, ví dụ player chết, level hoàn thành, item nhặt được.
- Khi muốn nối event giữa các scene khác nhau mà không phụ thuộc vào scene hierarchy.
- Khi team có designer cần tự cấu hình event flow mà không cần lập trình viên hỗ trợ.
- Không phù hợp cho event chỉ xảy ra cục bộ trong một component, dùng C# event hoặc delegate thông sẽ đơn giản hơn.
- Tránh dùng cho event tần suất cao mỗi frame, vì delegate invocation qua list có overhead cao hơn direct call.
- Không hỗ trợ return value, nếu cần response phải dùng pattern phức tạp hơn hoặc callback riêng.
- Decoupling hoàn toàn giữa publisher và subscriber, không cần reference trực tiếp tới `GameObject` hay `MonoBehaviour` cụ thể.
- Designer có thể kéo thả event asset vào Inspector để nối hệ thống mà không cần chỉnh code.

## Example
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
public class UIGameOver : MonoBehaviour
{
    public GameEvent onPlayerDied;
    public GameObject gameOverPanel;
    void OnEnable() => onPlayerDied.Register(ShowGameOver);
    void OnDisable() => onPlayerDied.Unregister(ShowGameOver);
    void ShowGameOver() => gameOverPanel.SetActive(true);
}
```

## Related notes
- [[Definition]]
- [[Runtime Set]]
- [[Data Persistence]]
- [[Summary]]
