---
aliases:
  - Update FixedUpdate LateUpdate
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Update Loop` tách frame thành ba nhịp chính: physics cố định, logic theo rendered frame, và logic chạy sau movement.

## Key points
- `FixedUpdate` chạy theo `Time.fixedDeltaTime`, mặc định `0.02s` tương đương 50 bước/giây.
- `FixedUpdate` có thể chạy 0, 1, hoặc nhiều lần trong một rendered frame tùy frame rate và nhu cầu physics.
- `Update` chạy mỗi rendered frame nếu component enabled. Dùng `Time.deltaTime` cho logic theo thời gian thực.
- `LateUpdate` chạy sau tất cả `Update`; đây là nơi tốt cho camera follow, aim correction, hoặc UI bám theo target đã di chuyển.
- Đọc input trong `Update`, lưu input vào field, rồi dùng field đó trong `FixedUpdate` nếu input ảnh hưởng physics.
- `WaitForFixedUpdate` resume sau fixed step; `WaitForEndOfFrame` resume cuối frame; `yield null` thường resume frame sau.

## Decision rules
- `FixedUpdate`: Force, velocity, `Rigidbody.MovePosition`, physics query gắn với simulation.
- `Update`: Input, non-physics movement, timers, AI tick nhẹ, animation trigger.
- `LateUpdate`: Camera, follow object, correction cần chạy sau movement.
- Không đọc one-frame input trong `FixedUpdate`; có thể bị miss.
- Không chạy logic nặng mỗi frame nếu có thể dùng event, timer, dirty flag, hoặc coroutine.
- Movement trong `Update`/`LateUpdate` nhân `Time.deltaTime`; physics movement dùng `Time.fixedDeltaTime` khi tự tính displacement.

## Example
```csharp
using UnityEngine;
public class PlayerController : MonoBehaviour
{
    public float speed = 5f;
    private Rigidbody _rb;
    private Vector3 _input;
    void Awake() => _rb = GetComponent<Rigidbody>();
    void Update()
    {
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");
        _input = new Vector3(h, 0, v).normalized;
    }
    void FixedUpdate()
    {
        _rb.MovePosition(_rb.position + _input * speed * Time.fixedDeltaTime);
    }
}
public class CameraFollow : MonoBehaviour
{
    public Transform target;
    public Vector3 offset = new Vector3(0, 10, -5);
    void LateUpdate()
    {
        transform.position = target.position + offset;
        transform.LookAt(target);
    }
}
```

## Related notes
- [[Definition]]
- [[Execution Order]]
- [[Initialization]]
- [[Decommissioning]]
- [[Summary]]
