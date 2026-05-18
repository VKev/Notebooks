---
aliases:
  - Pool State Management
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Pool Best Practices` là tập hợp các nguyên tắc để quản lý state, scene transition, và lifetime của pooled object đúng cách.

## Key points
- Unity `6.3`: quản lý state là thách thức lớn nhất khi dùng object pooling, vì object tái sử dụng có thể mang theo state cũ từ lần dùng trước.
- Ngoài state, cần xử lý pool khi chuyển scene, tránh reference leak, và quyết định thời điểm reset state.
- Reset state có hai cách tiếp cận: reset trong `actionOnGet` khi lấy ra, hoặc reset trong `actionOnRelease` khi trả về, cách nào cũng được miễn là nhất quán.
- Các state cần reset gồm: velocity trên `Rigidbody`, animation state, coroutine đang chạy, event subscription, particle system, timer, và `gameObject.SetActive`.
- Khi chuyển scene, object trong pool bị destroy nếu không có `DontDestroyOnLoad` trên pool manager.
- Nếu pool persist qua scene, object trả về phải được `SetParent` về pool root để không bị destroy cùng scene cũ.
- Collection như `List<T>` nên dùng `Clear()` thay vì allocate mới, vì `Clear()` giữ lại allocated memory.

## Decision rules
- State cũ là nguyên nhân phổ biến nhất gây bug khó tìm trong hệ thống pool: projectile bay sai hướng, enemy giữ HP cũ, UI hiển thị data sai.
- Quản lý scene đúng cách tránh `MissingReferenceException` khi pool reference object đã bị destroy bởi scene unload.
- Luôn áp dụng khi dùng bất kỳ hệ thống pooling nào, dù là `ObjectPool<T>` hay pool tự viết.
- Đặc biệt quan trọng khi object có nhiều state: `Rigidbody`, `Animator`, `ParticleSystem`, hoặc script tự quản lý timer và coroutine.
- Nếu object stateless hoặc state được ghi đè hoàn toàn mỗi lần dùng, không cần reset tường minh.
- Nếu game chỉ có một scene duy nhất, không cần lo chuyện scene transition cho pool.
- Không có cách tự động phát hiện state nào cần reset, phải track thủ công mỗi khi thêm feature mới cho pooled object.
- `DontDestroyOnLoad` làm pool tồn tại vĩnh viễn, cần quản lý cleanup thủ công khi không cần nữa.
- Object trong pool vẫn chiếm memory, cần gọi `pool.Clear()` giữa các level nếu pool quá lớn.

## Example
```csharp
using UnityEngine;
using UnityEngine.Pool;
public class Projectile : MonoBehaviour
{
    private Rigidbody _rb;
    private ObjectPool<GameObject> _pool;
    private float _timer;
    void Awake() => _rb = GetComponent<Rigidbody>();
    public void Init(ObjectPool<GameObject> pool, Vector3 direction, float speed)
    {
        _pool = pool;
        _timer = 3f;
        _rb.linearVelocity = Vector3.zero;
        _rb.angularVelocity = Vector3.zero;
        _rb.AddForce(direction * speed, ForceMode.VelocityChange);
    }
    void Update()
    {
        _timer -= Time.deltaTime;
        if (_timer <= 0f)
            _pool.Release(gameObject); // trả về pool thay vì Destroy
    }
    void OnCollisionEnter(Collision other)
    {
        _pool.Release(gameObject);
    }
}
```

## Related notes
- [[Definition]]
- [[ObjectPool API]]
- [[Summary]]
