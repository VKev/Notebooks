---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.4 (6000.4)`: mốc tài liệu chính cho API và behavior của `ScriptableObject`.
- API cần nhớ: `CreateInstance`, `CreateAssetMenu`, và lifecycle messages.

## Core keywords
- `ScriptableObject`: `UnityEngine.Object` dạng asset, chứa data độc lập với `GameObject`.
- `Shared data`: Nhiều object reference cùng một asset, nên không duplicate memory cho config dùng chung.
- `CreateAssetMenu`: Attribute đăng ký class vào Assets > Create để tạo `.asset` nhanh. Tham số chính: `menuName`, `fileName`, `order`.
- `CreateInstance<T>()`: Là static method duy nhất để tạo `ScriptableObject` instance bằng code. Không bao giờ dùng `new` hoặc constructor trực tiếp vì sẽ bypass Unity initialization.

## Lifecycle
- `Awake`: Gọi khi instance được tạo lần đầu. Chỉ chạy một lần trong vòng đời của asset.
- `OnEnable`: Gọi khi object load, vào Play Mode, hoặc recompile. Dùng để reset runtime data về giá trị ban đầu.
- `OnDisable / OnDestroy`: Gọi khi object unload hoặc bị hủy. Dùng để cleanup resource nếu cần.
- `OnValidate`: Chỉ chạy trong Editor khi Inspector thay đổi hoặc script reload. Dùng để clamp và validate data.

## Data persistence
- `Editor`: Thay đổi qua Inspector tự động lưu, thay đổi qua script cần `EditorUtility.SetDirty()`. Data trong Play Mode persist sau khi thoát Play Mode.
- `Build`: Asset là read-only, mọi thay đổi runtime mất khi tắt application. Cần hệ thống save riêng nếu muốn persist player data.
- `Scene reference`: Reference tới scene object sẽ null khi scene unload. `ScriptableObject` asset không serialize được reference tới `GameObject` trong scene.

## Architecture patterns
- `Event Channel`: Dùng `ScriptableObject` làm kênh trung gian broadcast event giữa publisher và subscriber. Decoupling hoàn toàn, designer có thể kéo thả trong Inspector.
- `Runtime Set`: Dùng `ScriptableObject` chứa list các object đang active trong scene. Thay thế `FindObjectsByType` và singleton bằng cách tiếp cận data-driven.

## Decision rules
- `Cần chia sẻ config data giữa nhiều prefab hoặc scene`: Dùng `ScriptableObject` asset. Nhiều object reference cùng một asset, tránh duplicate memory.
- `Cần lưu save game hoặc player progress`: Không dùng `ScriptableObject` làm nơi lưu trữ cuối cùng. Dùng hệ thống save riêng như JSON, binary, hoặc PlayerPrefs.
- `Cần event decoupling giữa các hệ thống`: Dùng Event Channel pattern. Tốt cho cross-scene communication và team có designer.
- `Cần danh sách object đang active mà không dùng Find`: Dùng Runtime Set pattern. Object tự đăng ký và hủy đăng ký.
- `Cần logic per-frame`: Không đặt trong `ScriptableObject`, vì `Update` không được gọi. Dùng `MonoBehaviour` để điều khiển và đọc data từ `ScriptableObject`.

## Common traps
- `Dùng new để tạo ScriptableObject`: Sai. Phải dùng `ScriptableObject.CreateInstance<T>()`. Constructor trực tiếp bypass Unity initialization và gây lỗi.
- `Nghĩ data runtime sẽ tự lưu trong build`: Sai. Trong build, `ScriptableObject` asset là read-only. Cần serialize ra file nếu muốn persist data giữa các lần chạy.
- `Data đổi trong Editor Play Mode sẽ tồn tại`: Đúng nhưng dễ gây nhầm. Clone asset hoặc reset default nếu cần bảo vệ data gốc.
- `Reference GameObject trong scene từ ScriptableObject asset`: Sai. Reference sẽ null khi scene unload. `ScriptableObject` chỉ nên reference các asset khác, không phải scene object.
- `Đặt Update logic trong ScriptableObject`: Sai. `Update` không được gọi trên `ScriptableObject`. Dùng `MonoBehaviour` cho per-frame logic và đọc/ghi data từ `ScriptableObject`.
- `Quên hủy đăng ký listener trong Event Channel`: Gây null reference hoặc memory leak khi object bị destroy. Luôn `Unregister` trong `OnDisable` để đảm bảo cleanup đúng.

## Review questions

### `ScriptableObject` khác `MonoBehaviour` ở điểm cốt lõi nào?
- `ScriptableObject` tồn tại như asset độc lập trong project, không gắn vào `GameObject`, không có `Transform`, và không nhận per-frame callback như `Update`.

### Vì sao không dùng `new` để tạo `ScriptableObject`?
- Vì `new` bypass Unity initialization system, cần dùng `ScriptableObject.CreateInstance<T>()` để Unity quản lý lifecycle đúng cách.

### Data trên `ScriptableObject` có persist trong build không?
- Không. Trong build, asset là read-only, mọi thay đổi runtime mất khi tắt application. Cần hệ thống save riêng để lưu trữ.

### Khi nào nên dùng `ScriptableObject` thay vì static variable?
- Khi cần nhiều instance độc lập với giá trị khác nhau, cần Inspector support, hoặc muốn tránh tight coupling giữa các class.

### Event Channel pattern giải quyết vấn đề gì?
- Decouple publisher và subscriber bằng một asset trung gian, không cần reference trực tiếp.

### Runtime Set pattern thay thế gì?
- Thay `FindObjectsByType` và singleton bằng list asset; mỗi object tự đăng ký khi active.

## Related notes
- [[Scriptable Object]]
- [[Definition]]
- [[CreateAssetMenu]]
- [[Lifecycle]]
- [[Data Persistence]]
- [[Event Channel]]
- [[Runtime Set]]
- [[../Unity|Unity]]
