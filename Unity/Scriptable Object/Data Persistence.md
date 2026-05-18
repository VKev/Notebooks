---
aliases:
  - ScriptableObject Persistence
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `ScriptableObject` lưu data vĩnh viễn trong Editor nhưng chỉ tồn tại tạm thời trong runtime build.

## Key points
- Unity `6.4 (6000.4)`: `ScriptableObject` asset là file `.asset` được serialize trong project, giữ data qua các lần mở Editor.
- Trong Editor Play Mode, thay đổi data trên `ScriptableObject` sẽ tồn tại sau khi thoát Play Mode, khác với `MonoBehaviour` thông bị reset.
- Trong standalone build, `ScriptableObject` asset là read-only, mọi thay đổi runtime sẽ mất khi tắt application.
- Instance tạo bằng `ScriptableObject.CreateInstance<T>()` lúc runtime là tạm thời, không tự động gắn vào asset system.
- Khi bạn tạo asset qua `[CreateAssetMenu]` hoặc `AssetDatabase.CreateAsset()`, Unity serialize data vào file `.asset` trên disk.
- Thay đổi qua Inspector tự động lưu, nhưng thay đổi qua script cần gọi `EditorUtility.SetDirty()` để Unity biết asset đã thay đổi, và tùy chọn gọi `AssetDatabase.SaveAssets()` để ghi xuống disk ngay lập tức.
- Value type như `float`, `int`, `string`, `Vector3` persist qua scene load vì `ScriptableObject` không bị destroy khi chuyển scene.
- Reference tới `GameObject` hoặc `MonoBehaviour` trong scene sẽ null khi scene đó unload, vì asset không serialize được reference tới scene object.

## Decision rules
- Khi cần chia sẻ configuration data giữa nhiều scene, nhiều prefab.
- Khi data là authoring-time và không cần thay đổi vĩnh viễn lúc runtime.
- Khi muốn tránh duplicate bộ nhớ cho các giá trị mà nhiều instance dùng chung.
- Không dùng để lưu save game hay player progress, vì data không persist trong build mà cần hệ thống save riêng như JSON hoặc binary serialization.
- Không reference `GameObject` trong scene từ `ScriptableObject` asset, vì reference sẽ null khi scene unload.
- Data persist qua scene load mà không cần `DontDestroyOnLoad` hay static variable.
- Nhiều object có thể reference cùng một asset, tránh duplicate data và giảm memory.
- Thay đổi trong Editor Play Mode tồn tại giúp test và iterate nhanh hơn, nhưng cần cẩn thận vì có thể ghi đè data gốc.

## Example
```csharp
using UnityEngine;
[CreateAssetMenu(fileName = "GameConfig.asset", menuName = "Game/Config")]
public class GameConfig : ScriptableObject
{
    public float gravity = -9.81f;
    public int maxEnemies = 50;
    public Color skyColor = Color.cyan;
}
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
        if (FindObjectsByType<Enemy>(FindObjectsSortMode.None).Length < config.maxEnemies)
            SpawnEnemy();
    }
    void SpawnEnemy() { /* spawn logic */ }
}
```

## Related notes
- [[Definition]]
- [[CreateAssetMenu]]
- [[Lifecycle]]
- [[Event Channel]]
- [[Summary]]
