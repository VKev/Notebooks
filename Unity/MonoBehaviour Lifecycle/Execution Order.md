---
aliases:
  - Event Function Order
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Execution Order` là thứ tự cố định mà Unity gọi các event function trên `MonoBehaviour` mỗi frame và qua các giai đoạn khác nhau.

## Key points
- Unity `6.3`: Unity chia vòng đời `MonoBehaviour` thành nhiều phase chạy theo thứ tự cố định: Initialization, Physics, Input, Game Logic, Rendering, và Decommissioning.
- Mỗi phase chứa các callback cụ thể, và thứ tự giữa các phase được đảm bảo, nhưng thứ tự giữa các script trong cùng một phase thì không, trừ khi cấu hình `Script Execution Order`.
- Hiểu sai thứ tự là nguyên nhân phổ biến nhất gây bug khó debug trong Unity.
- Phase 1 `Initialization`: `Awake` → `OnEnable` → `Start`, chạy một lần khi object khởi tạo.
- Phase 2 `Physics`: `FixedUpdate` → physics simulation → `OnTriggerXXX` / `OnCollisionXXX`, chạy theo fixed timestep, có thể nhiều lần mỗi frame hoặc không lần nào.
- Phase 3 `Game Logic`: `Update` → coroutine yield `null` → `LateUpdate`, chạy mỗi frame.
- Phase 4 `Rendering`: `OnWillRenderObject` → `OnBecameVisible` / `OnBecameInvisible` → `OnPreRender` → `OnRenderObject` → `OnPostRender` → `OnRenderImage`, chạy mỗi frame cho mỗi camera.
- Phase 5 `GUI`: `OnGUI` có thể chạy nhiều lần mỗi frame tùy số GUI event.
- Phase 6 `Decommissioning`: `OnDisable` → `OnDestroy` → `OnApplicationQuit`, chạy khi object bị tắt hoặc hủy.
- Tất cả managed thread bị suspend trong một số phase để đảm bảo consistency.

## Decision rules
- Đặt code đúng callback tránh race condition và thứ tự sai giữa các hệ thống.
- Physics logic trong `FixedUpdate` đảm bảo kết quả ổn định bất kể frame rate.
- Camera follow trong `LateUpdate` đảm bảo chạy sau khi object đã di chuyển trong `Update`.
- Luôn tham khảo execution order khi debug hành vi bất liên quan đến timing.
- Dùng `Script Execution Order` settings khi cần một script chạy trước script khác trong cùng phase.
- Không cố nhồi tất cả logic vào `Update`, phân chia đúng giữa `FixedUpdate`, `Update`, và `LateUpdate`.
- Không dựa vào thứ tự mặc định giữa các script, vì Unity không đảm bảo thứ tự đó.
- Thứ tự giữa các `MonoBehaviour` trong cùng phase là không xác định trừ khi cấu hình thủ công.
- `OnGUI` là legacy API, UI Toolkit hoặc UGUI được khuyến khích thay thế.
- Coroutine yield point nằm xen kẽ giữa các phase, không phải lúc nào cũng trực quan.

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
