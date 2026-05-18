---
aliases:
  - ObjectPool<T>
  - UnityEngine.Pool
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `ObjectPool<T>` là class built-in trong `UnityEngine.Pool` cho phép tạo pool tái sử dụng object với các callback cho create, get, release, và destroy.

## Key points
- Unity `6.4`: `ObjectPool<T>` là stack-based pool implement `IObjectPool<T>`, có sẵn từ Unity 2021 trở đi.
- Constructor nhận các delegate: `createFunc` để tạo object mới khi pool rỗng, `actionOnGet` khi lấy object ra, `actionOnRelease` khi trả object về, và `actionOnDestroy` khi object vượt quá `maxSize`.
- Ngoài `ObjectPool<T>`, Unity còn cung cấp `ListPool<T>`, `HashSetPool<T>`, `DictionaryPool<TKey,TValue>`, và `CollectionPool<T>` để tái sử dụng collection thay vì allocate mới mỗi frame.
- Gọi `pool.Get()` để lấy object: nếu pool có sẵn thì trả về và gọi `actionOnGet`, nếu rỗng thì gọi `createFunc` tạo mới.
- Gọi `pool.Release(obj)` để trả object: nếu pool chưa đầy thì lưu lại và gọi `actionOnRelease`, nếu đầy thì gọi `actionOnDestroy`.
- `collectionCheck` bật kiểm tra double-release, throw exception nếu release cùng object hai lần, chỉ hoạt động trong Editor.
- `CountActive` theo dõi số object đang dùng, `CountInactive` theo dõi số object trong pool, `CountAll` là tổng.
- `pool.Clear()` hủy tất cả object inactive trong pool, gọi `actionOnDestroy` cho từng cái.

## Decision rules
- Khi object được tạo và hủy thường xuyên: projectile, particle effect, enemy, UI popup.
- Dùng `CollectionPool` khi cần `List` hoặc `Dictionary` tạm trong method mà không muốn allocate mới.
- Dùng `Get(out PooledObject<T>)` overload khi muốn tự động release bằng `using` statement.
- Không cần pool cho object chỉ tạo một lần và tồn tại suốt game, ví dụ player, camera, manager.
- Không dùng cho object có state phức tạp khó reset, vì state cũ có thể gây bug nếu không cleanup kỹ.
- Không thread-safe, chỉ gọi từ main thread.
- API chính thức của Unity, không cần viết pool thủ công.
- Delegate pattern cho phép customize behavior cho mọi loại object: `GameObject`, class C# thường, hoặc collection.

## Example
```csharp
using UnityEngine;
using UnityEngine.Pool;
public class ProjectilePool : MonoBehaviour
{
    public GameObject prefab;
    private ObjectPool<GameObject> _pool;
    void Awake()
    {
        _pool = new ObjectPool<GameObject>(
            createFunc: () => Instantiate(prefab),
            actionOnGet: obj => obj.SetActive(true),
            actionOnRelease: obj => obj.SetActive(false),
            actionOnDestroy: obj => Destroy(obj),
            collectionCheck: true,
            defaultCapacity: 20,
            maxSize: 100
        );
    }
    public GameObject Spawn(Vector3 position)
    {
        GameObject obj = _pool.Get();
        obj.transform.position = position;
        return obj;
    }
    public void Despawn(GameObject obj) => _pool.Release(obj);
}
```

## Related notes
- [[Definition]]
- [[Pool Best Practices]]
- [[Summary]]
