---
aliases:
  - ScriptableObject Persistence
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `ScriptableObject` lưu data vĩnh viễn trong Editor nhưng chỉ tồn tại tạm thời trong runtime build.

## What is it
- Theo tài liệu Unity `6.3 LTS (6000.3)`, `ScriptableObject` asset là file `.asset` được serialize trong project, giữ data qua các lần mở Editor.
- Trong Editor Play Mode, thay đổi data trên `ScriptableObject` sẽ tồn tại sau khi thoát Play Mode, khác với `MonoBehaviour` thông thường bị reset.
- Trong standalone build, `ScriptableObject` asset là read-only, mọi thay đổi runtime sẽ mất khi tắt application.
- Instance tạo bằng `ScriptableObject.CreateInstance<T>()` lúc runtime là tạm thời, không tự động gắn vào asset system.

## How it works
- Khi bạn tạo asset qua `[CreateAssetMenu]` hoặc `AssetDatabase.CreateAsset()`, Unity serialize data vào file `.asset` trên disk.
- Thay đổi qua Inspector tự động lưu, nhưng thay đổi qua script cần gọi `EditorUtility.SetDirty()` để Unity biết asset đã thay đổi, và tùy chọn gọi `AssetDatabase.SaveAssets()` để ghi xuống disk ngay lập tức.
- Value type như `float`, `int`, `string`, `Vector3` persist qua scene load vì `ScriptableObject` không bị destroy khi chuyển scene.
- Reference tới `GameObject` hoặc `MonoBehaviour` trong scene sẽ null khi scene đó unload, vì asset không serialize được reference tới scene object.

## Why use it
- Data persist qua scene load mà không cần `DontDestroyOnLoad` hay static variable.
- Nhiều object có thể reference cùng một asset, tránh duplicate data và giảm memory.
- Thay đổi trong Editor Play Mode tồn tại giúp test và iterate nhanh hơn, nhưng cần cẩn thận vì có thể ghi đè data gốc.

## When to use it
- Dùng khi cần chia sẻ configuration data giữa nhiều scene, nhiều prefab.
- Dùng khi data là authoring-time và không cần thay đổi vĩnh viễn lúc runtime.
- Dùng khi muốn tránh duplicate bộ nhớ cho các giá trị mà nhiều instance dùng chung.

## When to not use it
- Không dùng để lưu save game hay player progress, vì data không persist trong build mà cần hệ thống save riêng như JSON hoặc binary serialization.
- Không reference `GameObject` trong scene từ `ScriptableObject` asset, vì reference sẽ null khi scene unload.

## Limitations
- Trong build, mọi thay đổi runtime chỉ là tạm thời và mất khi application đóng.
- `EditorUtility.SetDirty()` và `AssetDatabase.SaveAssets()` chỉ hoạt động trong Editor, không dùng được trong build.
- Reference tới scene object gây type mismatch trong Inspector và null khi scene bị unload.

---

## Example code
```csharp
using UnityEngine;

[CreateAssetMenu(fileName = "GameConfig.asset", menuName = "Game/Config")]
public class GameConfig : ScriptableObject
{
    public float gravity = -9.81f;
    public int maxEnemies = 50;
    public Color skyColor = Color.cyan;
}

// Nhiều MonoBehaviour đọc shared config mà không duplicate data
public class LevelManager : MonoBehaviour
{
    public GameConfig config;

    void Start()
    {
        Physics.gravity = new Vector3(0, config.gravity, 0);
    }
}

public class EnemySpawner : MonoBehaviour
{
    public GameConfig config;

    void Update()
    {
        // Cùng reference tới một asset, không tốn thêm memory
        if (FindObjectsByType<Enemy>(FindObjectsSortMode.None).Length < config.maxEnemies)
            SpawnEnemy();
    }

    void SpawnEnemy() { /* spawn logic */ }
}
```

---

## Related notes
- [[Definition]]
- [[CreateAssetMenu]]
- [[Lifecycle]]
- [[Event Channel]]
- [[Summary]]
