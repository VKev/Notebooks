---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.3 LTS (6000.3)`
  Đây là version tài liệu chính được dùng để kiểm tra API và behavior của `ScriptableObject` trong section này.
  Các API như `CreateInstance`, `CreateAssetMenu`, và lifecycle messages được kiểm tra với tài liệu version này hoặc mới hơn.

## Core keywords
- `ScriptableObject`
  Là class kế thừa `UnityEngine.Object`, cho phép tạo asset chứa data độc lập với `GameObject`.
  Giá trị chính là data store, giúp nhiều object chia sẻ cùng data mà không duplicate memory.
- `CreateAssetMenu`
  Là attribute đăng ký class vào Assets > Create menu để tạo asset `.asset` nhanh trong Editor.
  Có ba tham số: `menuName`, `fileName`, `order`.
  `fileName` tùy chỉnh nên kết thúc bằng `.asset`, còn `menuName` dùng dấu `/` để tạo submenu.
- `CreateInstance<T>()`
  Là static method duy nhất để tạo `ScriptableObject` instance bằng code.
  Không bao giờ dùng `new` hoặc constructor trực tiếp vì sẽ bypass Unity initialization.

## Lifecycle
- `Awake`
  Gọi khi instance được tạo lần đầu.
  Chỉ chạy một lần trong vòng đời của asset.
- `OnEnable`
  Gọi khi object load, vào Play Mode, hoặc recompile.
  Thường dùng để reset runtime data về giá trị ban đầu.
- `OnDisable / OnDestroy`
  Gọi khi object unload hoặc bị hủy.
  Dùng để cleanup resource nếu cần.
- `OnValidate`
  Chỉ chạy trong Editor khi Inspector thay đổi hoặc script reload.
  Dùng để clamp và validate data.

## Data persistence
- `Editor`
  Thay đổi qua Inspector tự động lưu, thay đổi qua script cần `EditorUtility.SetDirty()`.
  Data trong Play Mode persist sau khi thoát Play Mode.
- `Build`
  Asset là read-only, mọi thay đổi runtime mất khi tắt application.
  Cần hệ thống save riêng nếu muốn persist player data.
- `Scene reference`
  Reference tới scene object sẽ null khi scene unload.
  `ScriptableObject` asset không serialize được reference tới `GameObject` trong scene.

## Architecture patterns
- `Event Channel`
  Dùng `ScriptableObject` làm kênh trung gian broadcast event giữa publisher và subscriber.
  Decoupling hoàn toàn, designer có thể kéo thả trong Inspector.
- `Runtime Set`
  Dùng `ScriptableObject` chứa list các object đang active trong scene.
  Thay thế `FindObjectsByType` và singleton bằng cách tiếp cận data-driven.

## Decision rules
- `Cần chia sẻ config data giữa nhiều prefab hoặc scene`
  Dùng `ScriptableObject` asset.
  Nhiều object reference cùng một asset, tránh duplicate memory.
- `Cần lưu save game hoặc player progress`
  Không dùng `ScriptableObject` làm nơi lưu trữ cuối cùng.
  Dùng hệ thống save riêng như JSON, binary, hoặc PlayerPrefs.
- `Cần event decoupling giữa các hệ thống`
  Dùng Event Channel pattern.
  Tốt cho cross-scene communication và team có designer.
- `Cần danh sách object đang active mà không dùng Find`
  Dùng Runtime Set pattern.
  Object tự đăng ký và hủy đăng ký.
- `Cần logic per-frame`
  Không đặt trong `ScriptableObject`, vì `Update` không được gọi.
  Dùng `MonoBehaviour` để điều khiển và đọc data từ `ScriptableObject`.

## Common traps
- `Dùng new để tạo ScriptableObject`
  Sai. Phải dùng `ScriptableObject.CreateInstance<T>()`.
  Constructor trực tiếp bypass Unity initialization và gây lỗi.
- `Nghĩ data runtime sẽ tự lưu trong build`
  Sai. Trong build, `ScriptableObject` asset là read-only.
  Cần serialize ra file nếu muốn persist data giữa các lần chạy.
- `Quên rằng data thay đổi trong Editor Play Mode sẽ tồn tại`
  Đúng nhưng dễ gây nhầm. Data gốc bị ghi đè sau khi thoát Play Mode.
  Nếu cần bảo vệ data gốc, clone asset hoặc dùng default value reset.
- `Reference GameObject trong scene từ ScriptableObject asset`
  Sai. Reference sẽ null khi scene unload.
  `ScriptableObject` chỉ nên reference các asset khác, không phải scene object.
- `Đặt Update logic trong ScriptableObject`
  Sai. `Update` không được gọi trên `ScriptableObject`.
  Dùng `MonoBehaviour` cho per-frame logic và đọc/ghi data từ `ScriptableObject`.
- `Quên hủy đăng ký listener trong Event Channel`
  Gây null reference hoặc memory leak khi object bị destroy.
  Luôn `Unregister` trong `OnDisable` để đảm bảo cleanup đúng.

## Interview questions and answers
### `ScriptableObject` khác `MonoBehaviour` ở điểm cốt lõi nào?
- `ScriptableObject` tồn tại như asset độc lập trong project, không gắn vào `GameObject`, không có `Transform`, và không nhận per-frame callback như `Update`.

### Vì sao không dùng `new` để tạo `ScriptableObject`?
- Vì `new` bypass Unity initialization system, cần dùng `ScriptableObject.CreateInstance<T>()` để Unity quản lý lifecycle đúng cách.

### Data trên `ScriptableObject` có persist trong build không?
- Không. Trong build, asset là read-only, mọi thay đổi runtime mất khi tắt application. Cần hệ thống save riêng để lưu trữ.

### Khi nào nên dùng `ScriptableObject` thay vì static variable?
- Khi cần nhiều instance độc lập với giá trị khác nhau, cần Inspector support, hoặc muốn tránh tight coupling giữa các class.

### Event Channel pattern giải quyết vấn đề gì?
- Nó decoupling hoàn toàn publisher và subscriber, cho phép broadcast event qua một asset trung gian mà không cần reference trực tiếp.

### Runtime Set pattern thay thế gì?
- Nó thay thế `FindObjectsByType` tốn kém và singleton pattern bằng cách để mỗi object tự đăng ký vào một `ScriptableObject` list khi active.

## Related notes
- [[Scriptable Object]]
- [[Definition]]
- [[CreateAssetMenu]]
- [[Lifecycle]]
- [[Data Persistence]]
- [[Event Channel]]
- [[Runtime Set]]
- [[../Unity|Unity]]
