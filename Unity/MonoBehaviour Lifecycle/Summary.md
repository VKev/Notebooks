---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.3`: version tài liệu chính cho execution order và lifecycle callback trong section này.

## Core keywords
- `Awake`: Gọi khi script instance load, trước mọi `Start`. Dùng để cache `GetComponent` và thiết lập nội bộ.
- `OnEnable`: Gọi sau `Awake` và mỗi khi object bật lại. Dùng để subscribe event, đối xứng với `OnDisable`.
- `Start`: Gọi một lần trước frame `Update` đầu tiên, sau tất cả `Awake`. Dùng để setup cross-object reference.
- `FixedUpdate`: Chạy theo fixed timestep `0.02s`, độc lập frame rate. Dùng cho physics: `AddForce`, `MovePosition`.
- `Update`: Chạy mỗi frame, tần suất phụ thuộc frame rate. Dùng cho input handling và game logic chính.
- `LateUpdate`: Chạy sau tất cả `Update` mỗi frame. Dùng cho camera follow và logic cần kết quả của `Update`.

## Execution phases
- `Initialization`: `Awake` → `OnEnable` → `Start`, chạy khi object khởi tạo. `Awake` của tất cả object xong trước khi bất kỳ `Start` nào chạy.
- `Physics`: `FixedUpdate` → simulation → `OnCollision/OnTrigger`. Có thể chạy nhiều lần mỗi frame hoặc không lần nào.
- `Game Logic`: `Update` → coroutine `yield null` → `LateUpdate`. Chạy đúng một lần mỗi frame.
- `Decommissioning`: `OnDisable` → `OnDestroy` → `OnApplicationQuit`. `OnApplicationQuit` không đáng tin trên mobile khi app bị kill.

## Decision rules
- `Cần cache GetComponent`: Đặt trong `Awake`. Không đọc reference từ object khác ở đây vì chúng có thể chưa `Awake`.
- `Cần reference tới object khác`: Đặt trong `Start`. An toàn vì mọi `Awake` đã hoàn tất.
- `Cần subscribe event`: Đặt trong `OnEnable`, unsubscribe trong `OnDisable`. Đảm bảo đối xứng để tránh leak.
- `Cần physics interaction`: Đặt trong `FixedUpdate`. Không đặt physics code trong `Update`.
- `Cần chạy sau khi object di chuyển`: Đặt trong `LateUpdate`. Camera follow là use case phổ biến nhất.

## Common traps
- `Đọc reference object khác trong Awake`: Sai. Object đó có thể chưa `Awake`, gây null reference. Dùng `Start` cho cross-object setup.
- `Đặt physics code trong Update`: Sai. Kết quả phụ thuộc frame rate và không ổn định. Dùng `FixedUpdate` cho mọi tương tác physics.
- `Quên nhân Time.deltaTime trong Update`: Gây movement phụ thuộc frame rate. Luôn nhân `Time.deltaTime` trong `Update` và `LateUpdate`.
- `Nghĩ FixedUpdate chạy đúng một lần mỗi frame`: Sai. Có thể chạy 0 hoặc nhiều lần tùy frame rate. lý do physics cần tách khỏi rendering logic.
- `Dựa vào OnApplicationQuit trên mobile`: Không đáng tin. User có thể swipe kill app mà callback không chạy. Save data quan trọng trong `OnApplicationPause` thay thế.

## Review questions

### Awake và Start khác nhau thế nào?
- `Awake` chạy khi script load dù disabled, dùng cho self-setup. `Start` chạy sau tất cả `Awake` và chỉ khi enabled, dùng cho cross-object setup.

### Vì sao physics code phải đặt trong FixedUpdate?
- Vì `FixedUpdate` chạy theo fixed timestep ổn định, còn `Update` phụ thuộc frame rate, đặt physics trong `Update` sẽ cho kết quả không nhất quán.

### LateUpdate dùng để làm gì?
- Dùng cho logic cần chạy sau khi tất cả `Update` hoàn tất, phổ biến nhất là camera follow để tránh jitter.

### OnEnable và OnDisable nên dùng cho gì?
- Subscribe event trong `OnEnable` và unsubscribe trong `OnDisable` để đảm bảo cleanup đối xứng mỗi khi object bật tắt.

### FixedUpdate có chạy đúng một lần mỗi frame không?
- Không. Nó có thể chạy 0 lần nếu frame rate rất cao hoặc nhiều lần nếu frame rate thấp, vì nó theo fixed timestep riêng.

## Related notes
- [[MonoBehaviour Lifecycle]]
- [[Definition]]
- [[Execution Order]]
- [[Initialization]]
- [[Update Loop]]
- [[Decommissioning]]
- [[../Unity|Unity]]
