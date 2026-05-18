---
aliases:
  - Event Function Order
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Execution Order` là thứ tự Unity gọi callback trong Player Loop. Phase lớn có thứ tự rõ, nhưng thứ tự giữa object/script cùng phase thường không được đảm bảo.

## Key points
- Unity `6.4`: `Awake` và `OnEnable` chạy trước `Start` khi scene load object active.
- `SceneManager.sceneLoaded` chạy sau `OnEnable` và trước `Start` cho object trong scene.
- `FixedUpdate` thuộc physics loop, có thể chạy 0, 1, hoặc nhiều lần trước một rendered frame.
- `Update` chạy mỗi frame khi component enabled; `LateUpdate` chạy sau tất cả `Update`.
- Coroutine resume tùy yield instruction: `WaitForFixedUpdate` sau fixed step, `WaitForEndOfFrame` cuối frame, `yield null` quanh vùng sau `Update`.
- Callback rendering kiểu `OnPreCull`, `OnPreRender`, `OnRenderImage` chủ yếu áp dụng Built-in Render Pipeline/camera callback. Với URP/HDRP, ưu tiên SRP event, renderer feature, hoặc render pass.
- `OnGUI` có thể chạy nhiều lần mỗi frame vì xử lý nhiều GUI event.
- Không thể dựa vào thứ tự callback giữa nhiều instance cùng một `MonoBehaviour` class nếu Unity không document hoặc bạn không cấu hình rõ.

## Decision rules
- Dùng `Awake` để chuẩn bị chính object; dùng `Start` khi cần object khác đã `Awake`.
- Dùng `FixedUpdate` cho physics, `Update` cho input/game state, `LateUpdate` cho camera hoặc follow logic.
- Dùng `Script Execution Order` chỉ khi thật sự cần thứ tự giữa các script class. Nếu có thể, truyền dependency rõ thay vì dựa vào timing.
- Debug bug timing bằng cách hỏi: callback này chạy **trước hay sau** object cần đọc, physics step, hoặc movement?
- Trong URP/HDRP, không dùng Built-in camera callback làm nền tảng kiến trúc render mới.

## Example
```csharp
using UnityEngine;
public class LifecycleDemo : MonoBehaviour
{
    void Awake()    => Debug.Log("1. Awake");
    void OnEnable() => Debug.Log("2. OnEnable");
    void Start()    => Debug.Log("3. Start");
    void FixedUpdate() => Debug.Log("4. FixedUpdate");
    void Update()     => Debug.Log("5. Update");
    void LateUpdate() => Debug.Log("6. LateUpdate");
    void OnDisable() => Debug.Log("7. OnDisable");
    void OnDestroy() => Debug.Log("8. OnDestroy");
}
```

## Related notes
- [[Definition]]
- [[Initialization]]
- [[Update Loop]]
- [[Decommissioning]]
- [[Summary]]
