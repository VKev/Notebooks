---
aliases:
  - Addressable Loading
  - Ref-counting
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Load and Release` là cơ chế ref-counting của Addressables, trong đó mỗi lần load tăng ref-count và mỗi lần release giảm, asset chỉ unload khi ref-count về zero.

## Key points
- Theo tài liệu package `com.unity.addressables` version `2.10`, mỗi lần gọi load API sẽ tăng ref-count cho asset và AssetBundle chứa nó, và mỗi lần gọi `Release` sẽ giảm ref-count.
- Khi ref-count của asset về zero, asset sẵn sàng unload, nhưng AssetBundle chứa nó chỉ unload khi tất cả asset bên trong đều có ref-count zero.
- API chính gồm `Addressables.LoadAssetAsync<T>` cho single asset, `Addressables.LoadAssetsAsync<T>` cho nhiều asset, `Addressables.InstantiateAsync` cho instantiate trực tiếp, và `Addressables.LoadSceneAsync` cho scene.
- `LoadAssetAsync<T>(address)` trả về `AsyncOperationHandle<T>`, khi hoàn tất thì `handle.Result` chứa asset.
- `InstantiateAsync(address)` load và instantiate `GameObject` trong một lần gọi, trả về handle tới instance.
- Mỗi handle phải được release bằng `Addressables.Release(handle)` khi không cần nữa.
- Object instantiate bằng `Addressables.InstantiateAsync` phải release bằng `Addressables.ReleaseInstance(instance)`, không phải `Destroy`.
- Nếu instantiate bằng `Object.Instantiate` thông sau khi load, Unity không track instance đó, phải release handle gốc thủ công và tự `Destroy` instance.
- `AsyncOperationHandle` có thể dùng với `await` trong async method hoặc `yield return` trong coroutine.

## Decision rules
- Ref-counting đảm bảo asset không bị unload khi vẫn còn hệ thống cần dùng.
- Kiểm soát chính xác khi nào memory được giải phóng, tránh memory leak.
- Hỗ trợ async loading tránh frame spike khi load asset lớn.
- Luôn mirror mỗi load call với một release call tương ứng.
- Dùng `InstantiateAsync` khi cần spawn object để Addressables tự track instance.
- Dùng `LoadSceneAsync` khi scene là Addressable để Addressables quản lý dependency.
- Không gọi `Release` trước khi destroy instance, vì sẽ unload data trong khi instance vẫn cần.
- Không dùng `Object.Instantiate` cho Addressable asset nếu muốn Addressables track ref-count tự động.
- Không thể partial unload AssetBundle: một asset trong bundle chưa release sẽ giữ toàn bộ bundle trong memory.
- `InstantiateAsync` mỗi lần gọi tạo handle riêng, nếu spawn nhiều object phải release từng handle.
- Nếu release handle trước khi destroy instance, asset data bị unload và instance mất mesh, texture, hoặc material.

## Example
```csharp
using UnityEngine;
using UnityEngine.AddressableAssets;
using UnityEngine.ResourceManagement.AsyncOperations;
public class AssetLoader : MonoBehaviour
{
    private AsyncOperationHandle<GameObject> _prefabHandle;
    private GameObject _instance;
    public async void LoadAndSpawn(string address, Vector3 pos)
    {
        _prefabHandle = Addressables.LoadAssetAsync<GameObject>(address);
        await _prefabHandle.Task;
        if (_prefabHandle.Status == AsyncOperationStatus.Succeeded)
            _instance = Instantiate(_prefabHandle.Result, pos, Quaternion.identity);
    }
    public async void SpawnTracked(string address, Vector3 pos)
    {
        var handle = Addressables.InstantiateAsync(address, pos, Quaternion.identity);
        _instance = await handle.Task;
    }
    void OnDestroy()
    {
        if (_instance != null)
            Destroy(_instance);
        if (_prefabHandle.IsValid())
            Addressables.Release(_prefabHandle);
    }
}
```

## Related notes
- [[Definition]]
- [[AssetReference]]
- [[Addressable Group]]
- [[Summary]]
