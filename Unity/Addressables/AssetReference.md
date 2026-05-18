---
aliases:
  - Addressable AssetReference
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `AssetReference` là kiểu field cho phép reference tới Addressable asset trong Inspector mà không tự động load, cho phép kiểm soát thời điểm load và unload.

## Key points
- Theo tài liệu package `com.unity.addressables` version `2.10`, `AssetReference` lưu GUID của asset thay vì direct reference, giúp asset không bị load vào memory cho đến khi code chủ động gọi load.
- Unity cung cấp các subclass typed như `AssetReferenceGameObject`, `AssetReferenceTexture`, `AssetReferenceSprite` để enforce type safety trong Inspector.
- `AssetReferenceUILabelRestriction` attribute giới hạn chỉ cho phép gán asset có label cụ thể.
- Khai báo `AssetReference` field trong `MonoBehaviour` hoặc `ScriptableObject`, Unity hiển thị picker trong Inspector chỉ cho phép chọn Addressable asset.
- Gọi `assetRef.LoadAssetAsync<T>()` để bắt đầu load, trả về `AsyncOperationHandle<T>`.
- Khi load xong, truy cập `handle.Result` để lấy asset.
- `AssetReference` cache handle nội bộ, nên gọi `LoadAssetAsync` lần hai trên cùng instance sẽ không tạo load request mới.
- Gọi `assetRef.ReleaseAsset()` để giải phóng asset khi không cần nữa.

## Decision rules
- Khi asset không cần load ngay lập tức, ví dụ prefab enemy chỉ load khi player tới vùng đó.
- Khi muốn kiểm soát chính xác thời điểm asset vào và ra khỏi memory.
- Dùng thay thế direct reference cho asset lớn như texture, model, audio clip.
- Không cần cho asset nhỏ luôn cần thiết ngay từ đầu, direct reference đơn giản hơn.
- Tránh nếu project không dùng Addressables system.
- Tách biệt reference và loading: asset không tự động load khi scene mở, giảm memory và load time.
- Type-safe: compiler và Inspector đều kiểm tra kiểu, tránh load sai loại asset.
- Designer-friendly: kéo thả asset vào Inspector giống direct reference nhưng với deferred loading.

## Example
```csharp
using UnityEngine;
using UnityEngine.AddressableAssets;
using UnityEngine.ResourceManagement.AsyncOperations;
public class EnemySpawner : MonoBehaviour
{
    public AssetReferenceGameObject enemyPrefab;
    private AsyncOperationHandle<GameObject> _handle;
    public void SpawnEnemy(Vector3 position)
    {
        _handle = enemyPrefab.LoadAssetAsync<GameObject>();
        _handle.Completed += handle =>
        {
            if (handle.Status == AsyncOperationStatus.Succeeded)
                Instantiate(handle.Result, position, Quaternion.identity);
        };
    }
    void OnDestroy()
    {
        if (_handle.IsValid())
            Addressables.Release(_handle);
    }
}
```

## Related notes
- [[Definition]]
- [[Addressable Group]]
- [[Load and Release]]
- [[Summary]]
