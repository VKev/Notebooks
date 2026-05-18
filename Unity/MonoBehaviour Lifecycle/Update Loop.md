---
aliases:
  - Update FixedUpdate LateUpdate
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Update Loop` gồm `FixedUpdate`, `Update`, và `LateUpdate`, là ba callback per-frame chạy ở các thời điểm khác nhau để xử lý physics, game logic, và post-processing.

## What is it
- Theo tài liệu Unity `6.3`, `FixedUpdate` chạy theo fixed timestep mặc định `0.02s`, độc lập với frame rate, dùng cho physics.
- `Update` chạy một lần mỗi frame, dùng cho input và game logic chính.
- `LateUpdate` chạy một lần mỗi frame sau tất cả `Update`, dùng cho camera follow và logic cần kết quả của `Update`.

## How it works
- `FixedUpdate` có thể chạy nhiều lần trong một frame nếu frame rate thấp, hoặc không chạy lần nào nếu frame rate rất cao, để đảm bảo physics simulation ổn định.
- `Update` chạy đúng một lần mỗi frame, `Time.deltaTime` thay đổi theo frame rate thực tế.
- `LateUpdate` chạy sau khi tất cả `Update` của mọi script hoàn tất, đảm bảo mọi object đã di chuyển xong.
- Trong `FixedUpdate` không cần nhân với `Time.deltaTime` vì interval đã cố định, nhưng trong `Update` và `LateUpdate` cần dùng `Time.deltaTime` để movement mượt bất kể frame rate.
- Coroutine `yield return null` chạy giữa `Update` và `LateUpdate`, `yield return new WaitForFixedUpdate()` chạy sau `FixedUpdate`.

## Why use it
- Tách physics khỏi rendering logic đảm bảo simulation ổn định trên mọi thiết bị.
- Camera follow trong `LateUpdate` không bị jitter vì luôn chạy sau khi target đã di chuyển.
- Input trong `Update` đảm bảo responsive với user action mỗi frame.

## When to use it
- Dùng `FixedUpdate` cho `Rigidbody.AddForce`, `Rigidbody.MovePosition`, và mọi tương tác physics.
- Dùng `Update` cho input handling, animation trigger, game state logic.
- Dùng `LateUpdate` cho camera follow, UI update phụ thuộc vào vị trí object, IK correction.

## When to not use it
- Không đặt physics code trong `Update`, vì kết quả sẽ phụ thuộc frame rate và không ổn định.
- Không đặt input code trong `FixedUpdate`, vì có thể bỏ lỡ input giữa hai lần `FixedUpdate`.
- Không đặt logic nặng trong cả ba callback nếu không cần, vì chúng chạy mỗi frame.

## Limitations
- `FixedUpdate` không sync với frame, nên visual update trong `FixedUpdate` có thể gây jitter nếu không interpolate.
- `Time.deltaTime` trong `FixedUpdate` trả về `Time.fixedDeltaTime`, không phải thời gian thực giữa hai frame.
- Không có cách kiểm soát thứ tự `Update` giữa các script ngoại trừ `Script Execution Order`.

---

## Example code
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
        // Input: đọc mỗi frame
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");
        _input = new Vector3(h, 0, v).normalized;
    }

    void FixedUpdate()
    {
        // Physics: áp dụng lực theo fixed timestep
        _rb.MovePosition(_rb.position + _input * speed * Time.fixedDeltaTime);
    }
}

public class CameraFollow : MonoBehaviour
{
    public Transform target;
    public Vector3 offset = new Vector3(0, 10, -5);

    void LateUpdate()
    {
        // Camera: chạy sau khi target đã di chuyển trong Update/FixedUpdate
        transform.position = target.position + offset;
        transform.LookAt(target);
    }
}
```

---

## Related notes
- [[Definition]]
- [[Execution Order]]
- [[Initialization]]
- [[Decommissioning]]
- [[Summary]]
