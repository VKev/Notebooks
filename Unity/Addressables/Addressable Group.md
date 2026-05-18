---
aliases:
  - Addressable Groups
  - Asset Group
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Addressable Group` là cách tổ chức Addressable asset thành nhóm để kiểm soát cách chúng được đóng gói vào AssetBundle và phân phối.

## What is it
- Theo tài liệu package `com.unity.addressables` version `2.10`, mỗi Addressable asset thuộc về đúng một group, và group quyết định cách asset được đóng gói vào AssetBundle khi build.
- Group có `Schema` để cấu hình build setting như pack mode (cùng bundle hay riêng bundle), compression, và đường dẫn load (local hay remote).
- `Label` là tag gắn lên asset để nhóm chúng logic mà không phụ thuộc vào group, cho phép load nhiều asset theo label trong một lần gọi.

## How it works
- Mở window `Window > Asset Management > Addressables > Groups` để quản lý group.
- Mỗi group có schema định nghĩa: pack mode (Pack Together, Pack Separately, Pack Together By Label), build path, load path, và compression.
- `Pack Together` gom tất cả asset trong group vào một AssetBundle, tốt cho asset thường dùng cùng nhau.
- `Pack Separately` tạo riêng bundle cho mỗi asset, tốt cho asset load độc lập.
- `Content Catalog` là file mapping giữa address và vị trí physical của asset, được tạo khi build.
- `Profiles` cho phép cấu hình nhiều bộ build/load path cho các môi trường khác nhau (local dev, staging, production).

## Why use it
- Kiểm soát granularity của AssetBundle: gom nhiều asset nhỏ giảm overhead loading, tách asset lớn cho phép unload riêng lẻ.
- Hỗ trợ remote content: group có thể cấu hình load từ CDN, cho phép update content mà không cần rebuild app.
- Label cho phép load nhóm asset theo logic game mà không phụ thuộc vào cấu trúc thư mục.

## When to use it
- Dùng khi cần kiểm soát cách asset được đóng gói và phân phối.
- Dùng khi có remote content cần update riêng biệt với app.
- Dùng label khi cần load tất cả asset thuộc một category, ví dụ tất cả asset có label `"level1"`.

## When to not use it
- Không cần cấu hình phức tạp cho project nhỏ, default group setting thường đủ.
- Không tạo quá nhiều group nhỏ, vì mỗi bundle có overhead memory riêng.

## Limitations
- Không thể partial unload AssetBundle: tất cả asset trong bundle phải unload cùng lúc.
- Quá nhiều bundle nhỏ tăng memory overhead và load request, quá ít bundle lớn giảm khả năng unload granular.
- Remote content cần infrastructure CDN và catalog update workflow riêng.

---

## Example code
```
Addressable Group được cấu hình trong Editor, không phải code:

1. Window > Asset Management > Addressables > Groups
2. Create New Group > Packed Assets
3. Kéo asset vào group
4. Cấu hình Schema:
   - Build Path: LocalBuildPath hoặc RemoteBuildPath
   - Load Path: LocalLoadPath hoặc RemoteLoadPath
   - Bundle Mode: Pack Together / Pack Separately
5. Gán Label cho asset: click cột Label, thêm "level1", "enemies"
6. Build: Addressables > Build > New Build > Default Build Script

Load theo label trong code:
```

```csharp
using UnityEngine;
using UnityEngine.AddressableAssets;
using UnityEngine.ResourceManagement.AsyncOperations;
using System.Collections.Generic;

public class LevelLoader : MonoBehaviour
{
    public void LoadLevelAssets(string label)
    {
        Addressables.LoadAssetsAsync<GameObject>(
            label,
            obj => Debug.Log($"Loaded: {obj.name}")
        ).Completed += handle =>
        {
            if (handle.Status == AsyncOperationStatus.Succeeded)
                Debug.Log($"All assets with label '{label}' loaded");
        };
    }
}
```

---

## Related notes
- [[Definition]]
- [[AssetReference]]
- [[Load and Release]]
- [[Summary]]
