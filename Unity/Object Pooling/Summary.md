---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.4`: `ObjectPool<T>` có sẵn từ Unity 2021 trong namespace `UnityEngine.Pool`. API không thay đổi đáng kể qua các version.

## Core keywords
- `ObjectPool<T>`: Class built-in stack-based pool, implement `IObjectPool<T>`. Constructor nhận delegate cho create, get, release, destroy.
- `Get()`: Lấy object từ pool, tạo mới nếu pool rỗng. Gọi `actionOnGet` callback.
- `Release()`: Trả object về pool, destroy nếu pool đầy `maxSize`. Gọi `actionOnRelease` callback.
- `CollectionPool`: Pool có sẵn cho `List<T>`, `HashSet<T>`, `Dictionary<TKey,TValue>`. Dùng `Clear()` thay vì allocate mới mỗi frame.

## State management
- `Reset on get`: Reset state trong `actionOnGet` khi lấy object ra. Đảm bảo object sạch trước khi dùng.
- `Reset on release`: Reset state trong `actionOnRelease` khi trả object về. Đảm bảo pool chỉ chứa object đã cleanup.
- `State cần reset`: Velocity, animation, coroutine, event subscription, particle, timer. Quên reset là nguyên nhân phổ biến gây bug pool.

## Decision rules
- `Object tạo và hủy thường xuyên`: Pool nó. Projectile, particle, enemy, UI popup. Giảm GC pressure đáng kể.
- `Object tạo một lần tồn tại suốt game`: Không cần pool. Player, camera, manager. Pool thêm complexity mà không có lợi.
- `Cần List tạm trong method mỗi frame`: Dùng `ListPool<T>.Get()` và `Release()`. Tránh allocate collection mới mỗi frame.
- `Pool quá lớn giữa các level`: Gọi `pool.Clear()` giữa level. Object trong pool vẫn chiếm memory.

## Common traps
- `Quên reset state khi lấy object từ pool`: Object mang theo state cũ: velocity, HP, timer. Luôn reset trong `actionOnGet` hoặc method `Init`.
- `Dùng Destroy thay vì Release`: Object bị hủy thật thay vì trả về pool. Dùng `pool.Release(obj)` thay vì `Destroy(obj)`.
- `Pool không persist qua scene`: Object trong pool bị destroy khi scene unload. Dùng `DontDestroyOnLoad` cho pool manager nếu cần persist.
- `Release object hai lần`: Gây bug hoặc exception nếu `collectionCheck` bật. Track state để đảm bảo mỗi object chỉ release một lần.
- `Dùng ObjectPool từ background thread`: `ObjectPool<T>` không thread-safe. Chỉ gọi từ main thread.

## Review questions

### Object Pooling giải quyết vấn đề gì?
- Giảm GC pressure và CPU cost do `Instantiate`/`Destroy` liên tục, bằng cách tái sử dụng object thay vì tạo mới.

### ObjectPool<T> hoạt động thế nào?
- Stack-based pool: `Get()` lấy hoặc tạo object, `Release()` trả về pool. Có callback cho mỗi giai đoạn và giới hạn `maxSize`.

### Vì sao phải reset state khi lấy object từ pool?
- Vì object tái sử dụng có thể mang theo state cũ từ lần dùng trước, gây bug như velocity sai, HP cũ, hoặc animation sai.

### Khi nào không nên dùng pooling?
- Khi object tạo ít và tồn tại lâu, hoặc khi state quá phức tạp để reset đúng, hoặc khi không nằm trong hot path.

## Related notes
- [[Object Pooling]]
- [[Definition]]
- [[ObjectPool API]]
- [[Pool Best Practices]]
- [[../Unity|Unity]]
