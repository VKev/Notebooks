---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `com.unity.addressables 2.10`: Package version chính được dùng trong section này. API có thể thay đổi nhẹ giữa các version.

## Core keywords
- `AssetReference`: Field type reference tới Addressable asset, deferred loading. Lưu GUID, không load cho đến khi gọi `LoadAssetAsync`.
- `Addressable Group`: Tổ chức asset thành nhóm, quyết định cách đóng gói AssetBundle. Mỗi asset thuộc đúng một group.
- `Label`: Tag gán lên asset để nhóm logic, load nhiều asset cùng label. Không phụ thuộc vào group.
- `Content Catalog`: File mapping giữa address và vị trí physical của asset. Tạo khi build, dùng để locate asset lúc runtime.

## Loading and releasing
- `LoadAssetAsync<T>`: Load single asset theo address, trả về `AsyncOperationHandle<T>`. Mỗi lần gọi tăng ref-count.
- `InstantiateAsync`: Load và instantiate `GameObject` trong một lần gọi. Addressables tự track instance cho ref-counting.
- `Release / ReleaseInstance`: Giảm ref-count, asset unload khi ref-count về zero. Phải mirror mỗi load với một release.
- `Ref-counting`: Cơ chế đếm số lần load để biết khi nào safe unload. AssetBundle chỉ unload khi tất cả asset bên trong đều ref-count zero.

## Decision rules
- `Asset lớn không cần ngay từ đầu`: Dùng `AssetReference` với deferred loading. Load khi cần, release khi xong.
- `Nhiều asset cùng category cần load cùng lúc`: Gán label và dùng `LoadAssetsAsync` theo label. Ví dụ: tất cả asset cho level 1.
- `Cần update content mà không rebuild app`: Cấu hình group với remote build/load path. Dùng content update workflow.
- `Cần spawn nhiều instance`: Dùng `InstantiateAsync` để Addressables track. Không dùng `Object.Instantiate` cho Addressable asset.

## Common traps
- `Dùng Object.Instantiate cho Addressable asset`: Addressables không track instance đó, ref-count sai. Dùng `Addressables.InstantiateAsync` thay thế.
- `Release handle trước khi destroy instance`: Asset data bị unload, instance mất mesh/texture/material. Luôn destroy instance trước, release handle sau.
- `Quên release handle`: AssetBundle không bao giờ unload, memory leak. Mirror mỗi load với một release tương ứng.
- `Nghĩ có thể partial unload AssetBundle`: Sai. Một asset chưa release giữ toàn bộ bundle trong memory. Tổ chức group hợp lý để asset cùng lifetime ở cùng bundle.
- `Gọi LoadAssetAsync trên AssetReference hai lần`: Handle bị cache, lần gọi thứ hai không tạo load mới. Dùng `Addressables.LoadAssetAsync` trực tiếp nếu cần nhiều load.

## Review questions

### Addressables giải quyết vấn đề gì so với Resources folder?
- Addressables hỗ trợ async loading, ref-counting, remote content, và không yêu cầu asset nằm trong folder cố định như `Resources`.

### Ref-counting hoạt động thế nào?
- Mỗi lần load tăng ref-count, mỗi lần release giảm. Asset chỉ unload khi ref-count về zero. AssetBundle chỉ unload khi tất cả asset bên trong đều zero.

### Vì sao không nên dùng Object.Instantiate cho Addressable asset?
- Vì Addressables không track instance đó, khi release handle gốc, asset data bị unload trong khi instance vẫn tồn tại, gây missing reference.

### AssetReference khác direct reference thế nào?
- Direct reference load asset ngay khi scene mở. `AssetReference` lưu GUID và chỉ load khi code gọi `LoadAssetAsync`, giảm memory và load time.

### Khi nào nên tách asset ra nhiều group?
- Khi asset có lifetime khác nhau, vì không thể partial unload bundle. Asset cùng lifetime nên ở cùng group.

## Related notes
- [[Addressables]]
- [[Definition]]
- [[AssetReference]]
- [[Addressable Group]]
- [[Load and Release]]
- [[../Unity|Unity]]
