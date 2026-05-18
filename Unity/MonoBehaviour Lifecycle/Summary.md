---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.4 (6000.4)`: mốc tài liệu chính cho execution order và lifecycle callback trong section này.

## Core keywords
- `Awake`: Self-setup một lần. Cache component, tạo collection, set invariant nội bộ.
- `OnEnable`: Bật kết nối tạm thời. Subscribe event, start listener, reset state mỗi lần active.
- `Start`: Cross-object setup một lần. Chạy sau mọi `Awake`, trước `Update` đầu tiên.
- `FixedUpdate`: Physics tick theo `Time.fixedDeltaTime`, mặc định `0.02s`.
- `Update`: Frame tick cho input, gameplay state, timer, non-physics logic.
- `LateUpdate`: After-Update tick cho camera follow, aim correction, UI bám target.
- `OnDisable`: Cleanup đối xứng với `OnEnable`. Unsubscribe, stop listener, trả state tạm.
- `OnDestroy`: Cleanup cuối khi object/component sắp bị destroy; không đảm bảo thứ tự giữa object.

## Execution phases
- `Initialization`: `Awake` → `OnEnable` → `sceneLoaded` → `Start`.
- `Physics`: `FixedUpdate` → physics simulation → collision/trigger callbacks.
- `Frame logic`: `Update` → coroutine resume tùy yield → `LateUpdate`.
- `Rendering`: Built-in camera callbacks hoặc SRP/URP/HDRP render flow.
- `Decommissioning`: `OnDisable` → `OnDestroy`; app-level save nên dùng pause/focus loss trên mobile.

## Decision rules
- `Self dependency`: `Awake`.
- `Other object dependency`: `Start` hoặc explicit injection.
- `Event subscription`: `OnEnable`/`OnDisable`.
- `Physics write`: `FixedUpdate`.
- `One-frame input`: `Update`.
- `Follow/camera/correction`: `LateUpdate`.
- `Important save data`: `OnApplicationPause(true)` hoặc `OnApplicationFocus(false)`, không chỉ `OnApplicationQuit`.

## Common traps
- `Awake đọc object khác`: Dễ race condition. Dùng `Start`, injection, hoặc event ready rõ ràng.
- `Input trong FixedUpdate`: Có thể miss input ngắn. Đọc trong `Update`, cache lại.
- `Physics trong Update`: Phụ thuộc frame rate. Ghi physics trong `FixedUpdate`.
- `OnEnable chứa one-time init`: Sai khi object bật/tắt nhiều lần. One-time init đặt trong `Awake` hoặc `Start`.
- `OnApplicationQuit để save mobile`: Không đáng tin. Save khi pause/focus loss.
- `Destroy order cố định`: Không. Cleanup phải chịu được manager/listener đã mất.

## Review questions

### Awake và Start khác nhau thế nào?
- `Awake` chạy một lần để setup chính object. `Start` chạy sau mọi `Awake` và chỉ khi enabled, nên hợp cho setup cần object khác.

### Vì sao physics code phải đặt trong FixedUpdate?
- Vì `FixedUpdate` bám theo physics timestep, còn `Update` bám theo rendered frame. Physics trong `Update` dễ lệch theo FPS.

### LateUpdate dùng để làm gì?
- Dùng cho logic cần kết quả sau movement, phổ biến nhất là camera follow hoặc correction sau `Update`.

### OnEnable và OnDisable nên dùng cho gì?
- Dùng cho kết nối tạm thời: subscribe/unsubscribe event, start/stop listener, reset/release state mỗi lần bật tắt.

### FixedUpdate có chạy đúng một lần mỗi frame không?
- Không. Nó có thể chạy 0, 1, hoặc nhiều lần trước một rendered frame vì physics có timestep riêng.

## Related notes
- [[MonoBehaviour Lifecycle]]
- [[Definition]]
- [[Execution Order]]
- [[Initialization]]
- [[Update Loop]]
- [[Decommissioning]]
- [[../Unity|Unity]]
