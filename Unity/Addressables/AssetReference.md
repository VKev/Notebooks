---
aliases:
  - Addressable AssetReference
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `AssetReference` là kiểu field cho phép reference tới Addressable asset trong Inspector mà không tự động load, cho phép kiểm soát thời điểm load và unload.

## What is it
- Theo tài liệu package `com.unity.addressables` version `2.10`, `AssetReference` lưu GUID của asset thay vì direct reference, giúp asset không bị load vào memory cho đến khi code chủ động gọi load.
- Unity cung cấp các subclass typed như `AssetReferenceGameObject`, `AssetReferenceTexture`, `AssetReferenceSprite` để enforce type safety trong Inspector.
- `AssetReferenceUILabelRestriction` attribute giới hạn chỉ cho phép gán asset có label cụ thể.

## How it works
- Khai báo `AssetReference` field trong `MonoBehaviour` hoặc `ScriptableObject`, Unity hiển thị picker trong Inspector chỉ cho phép chọn Addressable asset.
- Gọi `assetRef.LoadAssetAsync<T>()` để bắt đầu load, trả về `AsyncOperationHandle<T>`.
- Khi load xong, truy cập `handle.Result` để lấy asset.
- `AssetReference` cache handle nội bộ, nên gọi `LoadAssetAsync` lần hai trên cùng instance sẽ không tạo load request mới.
- Gọi `assetRef.ReleaseAsset()` để giải phóng asset khi không cần nữa.

## Why use it
- Tách biệt reference và loading: asset không tự động load khi scene mở, giảm memory và load time.
- Type-safe: compiler và Inspector đều kiểm tra kiểu, tránh load sai loại asset.
- Designer-friendly: kéo thả asset vào Inspector giống direct reference nhưng với deferred loading.

## When to use it
- Dùng khi asset không cần load ngay lập tức, ví dụ prefab enemy chỉ load khi player tới vùng đó.
- Dùng khi muốn kiểm soát chính xác thời điểm asset vào và ra khỏi memory.
- Dùng thay thế direct reference cho asset lớn như texture, model, audio clip.

## When to not use it
- Không cần cho asset nhỏ luôn cần thiết ngay từ đầu, direct reference đơn giản hơn.
- Không dùng nếu project không dùng Addressables system.

## Limitations
- `LoadAssetAsync` chỉ cache một handle, gọi lại không tăng ref-count, cần dùng `Addressables.LoadAssetAsync` trực tiếp nếu cần nhiều load.
- Phải release đúng cách, nếu quên release sẽ giữ asset và AssetBundle trong memory vĩnh viễn.
- `AssetReference` không hỗ trợ load đồng bộ, luôn phải dùng async.

---

## Example code
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
        // Release khi không cần nữa
        if (_handle.IsValid())
            Addressables.Release(_handle);
    }
}
```

---

## Related notes
- [[Definition]]
- [[Addressable Group]]
- [[Load and Release]]
- [[Summary]]
