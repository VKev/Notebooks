---
aliases:
  - Junior Unity Interview
  - Unity Junior Checklist
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Junior Interview` là nhóm kiến thức Unity nền tảng cần nắm chắc để trả lời phỏng vấn junior, làm task rõ yêu cầu, sửa bug cơ bản, và đọc hiểu project có sẵn.

## What is it
- Junior Unity developer cần hiểu cách Unity tổ chức game bằng `Scene`, `GameObject`, `Component`, `Prefab`, và `Asset`.
- Cần biết script Unity thường kế thừa `MonoBehaviour`, được gắn vào `GameObject`, và chạy thông qua lifecycle callback như `Awake`, `Start`, `Update`, `FixedUpdate`, `OnEnable`, `OnDisable`.
- Cần biết dùng Inspector, serialized field, prefab override, scene reference, và debug bằng Console/Breakpoint.

## Core topics
- `GameObject and Component`
  `GameObject` là container trong scene, còn behavior nằm trong component.
  Không nhồi mọi logic vào một script lớn nếu có thể tách responsibility rõ hơn.
- `Transform`
  Mọi `GameObject` trong scene có `Transform`.
  Cần phân biệt local position, world position, rotation, scale, và parent-child hierarchy.
- `Prefab`
  Prefab là template tái sử dụng cho object.
  Cần hiểu instance override, apply/revert, nested prefab, và variant ở mức cơ bản.
- `MonoBehaviour Lifecycle`
  `Awake` dùng để init internal reference, `Start` dùng khi cần object khác đã setup, `Update` chạy mỗi frame, `FixedUpdate` hợp với physics.
  Cleanup event hoặc reference trong `OnDisable` hoặc `OnDestroy`.
- `Serialization and Inspector`
  Dùng `[SerializeField] private` để expose config mà vẫn giữ field private.
  Không mong Unity serialize mọi type như dictionary trực tiếp.
- `Coroutine`
  Coroutine giúp chia logic theo thời gian mà không block main thread.
  Cần hiểu `yield return null`, `WaitForSeconds`, và coroutine không phải thread riêng.
- `Physics`
  Biết dùng `Rigidbody`, `Collider`, trigger, collision callback, layer collision matrix, và raycast cơ bản.
  Không di chuyển physics object bằng cách set `transform.position` tùy tiện trong mọi trường hợp.
- `UI`
  Hiểu Canvas, RectTransform, anchor, layout group, button event, và cách tránh UI rebuild không cần thiết ở mức cơ bản.
- `Debugging`
  Biết đọc Console error, stack trace, missing reference, null reference, và dùng breakpoint.
  Không sửa bằng đoán mò khi có thể tái hiện bug và khoanh vùng nguyên nhân.

## Practical expectations
- Làm được feature nhỏ theo spec rõ ràng.
- Sửa bug có reproduction cụ thể.
- Biết hỏi lại khi requirement thiếu input, output, hoặc edge case.
- Biết dùng prefab, scene, và asset mà không phá reference có sẵn.
- Biết commit thay đổi nhỏ, dễ review, không refactor lan rộng khi không cần.

## Common gaps
- Lạm dụng `FindObjectOfType`, `GameObject.Find`, hoặc singleton cho mọi dependency.
- Quên unsubscribe event khi object disable/destroy.
- Đặt logic nặng trong `Update` mà không kiểm soát tần suất.
- Không phân biệt scene object reference và asset reference.
- Sửa prefab instance nhưng quên apply hoặc tạo override ngoài ý muốn.
- Không release Addressables hoặc resource được load động.

## Interview signals
- Trả lời tốt khi giải thích được vì sao dùng `Awake` hay `Start`.
- Trả lời tốt khi biết coroutine không chạy trên thread khác.
- Trả lời tốt khi biết `Update` phụ thuộc frame rate, còn `FixedUpdate` theo physics timestep.
- Trả lời tốt khi biết private field có thể hiện Inspector bằng `[SerializeField]`.
- Trả lời tốt khi biết đọc lỗi `NullReferenceException` từ stack trace.

---

## Interview questions and answers
### `GameObject` và `Component` khác nhau thế nào?
- `GameObject` là object/container trong scene. `Component` là phần thêm dữ liệu hoặc hành vi cho `GameObject`, ví dụ `Transform`, `Rigidbody`, `Collider`, hoặc script kế thừa `MonoBehaviour`.

### `MonoBehaviour` là gì?
- `MonoBehaviour` là base class cho script Unity gắn vào `GameObject`, cho phép nhận lifecycle callback như `Awake`, `Start`, `Update`, `OnEnable`, và `OnDisable`.

### `Awake` và `Start` khác nhau thế nào?
- `Awake` chạy khi object được load hoặc instance được tạo, thường dùng init reference nội bộ. `Start` chạy trước frame update đầu tiên khi component enabled, thường dùng khi cần object khác đã `Awake` xong.

### `Update` và `FixedUpdate` khác nhau thế nào?
- `Update` chạy mỗi frame, phù hợp input và logic frame-based. `FixedUpdate` chạy theo fixed timestep, phù hợp physics như thao tác với `Rigidbody`.

### Coroutine có phải thread riêng không?
- Không. Coroutine vẫn chạy trên main thread. Nó chỉ cho phép tạm dừng và tiếp tục execution qua `yield`, ví dụ chờ frame tiếp theo hoặc chờ thời gian.

### Vì sao nên dùng `[SerializeField] private`?
- Vì field vẫn chỉnh được trong Inspector nhưng không bị biến thành public API cho code khác ghi trực tiếp. Đây là cách giữ encapsulation tốt hơn.

### Prefab dùng để làm gì?
- Prefab là template tái sử dụng cho `GameObject` và component. Nó giúp tạo nhiều instance nhất quán, dễ apply/revert thay đổi, và giảm lặp setup trong scene.

### `Rigidbody` và `Collider` khác nhau thế nào?
- `Collider` định nghĩa vùng va chạm. `Rigidbody` cho object tham gia physics simulation như gravity, force, velocity, và collision response.

### `OnCollisionEnter` và `OnTriggerEnter` khác nhau thế nào?
- `OnCollisionEnter` dùng cho va chạm vật lý bình thường. `OnTriggerEnter` dùng khi collider bật `Is Trigger`, chỉ phát hiện overlap và thường không tạo phản lực vật lý.

### Lỗi `NullReferenceException` nghĩa là gì?
- Code đang truy cập member trên một reference đang `null`. Cần đọc stack trace, tìm dòng lỗi, kiểm tra object đã assign trong Inspector hay đã được khởi tạo chưa.

### Scene reference và asset reference khác nhau thế nào?
- Scene reference trỏ tới object nằm trong scene hiện tại. Asset reference trỏ tới asset trong project như prefab, texture, material, hoặc `ScriptableObject`.

### Local position và world position khác nhau thế nào?
- `world position` là vị trí trong hệ tọa độ thế giới. `localPosition` là vị trí tương đối so với parent. Nếu object có parent, giá trị `Transform.position` và `Transform.localPosition` có thể khác nhau.

### Euler angle và Quaternion khác nhau thế nào?
- Euler angle dễ đọc vì dùng ba góc X/Y/Z nhưng có thể gặp gimbal lock và interpolation khó ổn định. Quaternion khó nhìn trực tiếp hơn nhưng phù hợp để biểu diễn rotation nội bộ và nội suy mượt như `Slerp`.

### Canvas có những render mode phổ biến nào?
- `Screen Space - Overlay` vẽ UI trực tiếp lên màn hình, `Screen Space - Camera` vẽ qua camera, còn `World Space` đặt UI như object trong world. Chọn mode theo việc UI thuộc HUD, camera-specific UI, hay UI trong thế giới 3D.

### Khi nào dùng `ScriptableObject`?
- Dùng cho data/config dùng chung giữa nhiều object hoặc nhiều scene, ví dụ item data, enemy stats, ability config. Không dùng nó làm save game store cuối cùng trong build.

### Vì sao không nên dùng `GameObject.Find` trong hot path?
- Vì nó tìm object theo scene hierarchy và có thể tốn chi phí nếu gọi lặp lại, đặc biệt trong `Update`. Nên cache reference, inject dependency, hoặc dùng reference qua Inspector.

### Khi nào dùng object pooling?
- Dùng khi object được tạo/hủy lặp lại nhiều lần như bullet, VFX, enemy spawn. Pooling giảm allocation, giảm GC pressure, và tránh spike do instantiate/destroy liên tục.

### `Time.deltaTime` dùng để làm gì?
- Dùng để scale movement hoặc timer theo thời gian thực giữa frame, giúp logic ít phụ thuộc FPS hơn.

### Khi nào dùng `FixedUpdate` nhưng vẫn đọc input trong `Update`?
- Input nên đọc trong `Update` để không miss frame input. Physics action có thể lưu input state rồi xử lý trong `FixedUpdate`.

### Điều chỉnh `Time.fixedDeltaTime` ảnh hưởng gì?
- Fixed timestep nhỏ hơn giúp physics cập nhật thường xuyên hơn và có thể mượt/chính xác hơn, nhưng tăng CPU cost. Fixed timestep lớn hơn giảm CPU cost nhưng dễ giảm độ ổn định hoặc làm chuyển động vật lý kém chính xác.

### Vật thể bay nhanh xuyên collider thì xử lý thế nào?
- Kiểm tra collision detection mode, dùng `Continuous Speculative`, `Continuous`, hoặc `Continuous Dynamic` cho object thật sự cần. Đồng thời profile vì continuous collision detection tốn CPU hơn `Discrete`.

### Draw call là gì và vì sao quan trọng?
- Draw call là lệnh CPU gửi tới GPU để vẽ. Quá nhiều draw call có thể làm CPU render thread tốn thời gian, nhất là trên mobile hoặc scene nhiều material/object.

### `DrawCall`, `Batch`, và `SetPass Call` khác nhau thế nào?
- `DrawCall` là lệnh vẽ gửi tới GPU. `Batch` là nhóm object được gom để giảm số lần gửi lệnh. `SetPass Call` là lần đổi pass/material state; số `SetPass` cao thường báo hiệu shader/material state thay đổi nhiều.

### Batching và GPU Instancing khác nhau thế nào?
- Batching cố gộp nhiều object thành ít draw call hơn theo rule nhất định. GPU Instancing vẽ nhiều instance cùng mesh/material bằng một draw call hoặc ít draw call hơn.

### Vì sao `Instantiate` và `Destroy` nhiều lần gây tốn performance?
- Vì tạo/hủy object kích hoạt allocation, setup component, lifecycle callback, native object work, và sau đó tạo áp lực GC. Object pooling thường tốt hơn cho object xuất hiện lặp lại như bullet hoặc VFX.

### Vì sao LINQ có thể nguy hiểm trong `Update`?
- LINQ có thể tạo allocation, iterator, delegate, hoặc chạy query nhiều lần. Trong hot path mỗi frame, loop thường dễ kiểm soát performance hơn.

### Làm sao implement physics trong game 2D Unity?
- Dùng `Rigidbody2D` cho object cần chịu lực hoặc gravity, dùng `Collider2D` để định nghĩa vùng va chạm, cấu hình layer collision matrix, rồi xử lý qua callback như `OnCollisionEnter2D` hoặc `OnTriggerEnter2D`.

### Làm sao xử lý touch input hoặc accelerometer trên mobile?
- Đọc touch bằng `Input.touchCount` và `Input.GetTouch`, kiểm tra phase như began/moved/ended. Với accelerometer, đọc `Input.acceleration`, thường nên lọc hoặc smooth dữ liệu trước khi dùng cho gameplay.

### Làm sao làm UI không phụ thuộc resolution và aspect ratio?
- Dùng `Canvas Scaler` với reference resolution phù hợp, chỉnh match width/height, dùng anchor/pivot đúng cho từng UI element, và test nhiều tỉ lệ màn hình thay vì chỉ một resolution trong Editor.

### Animator Controller hoạt động thế nào?
- `Animator Controller` quản lý các animation state và transition giữa chúng. Script thường set parameter như `speed`, `isGrounded`, hoặc `attackTrigger` để state machine chuyển animation đúng thời điểm.

### Làm sao tạo enemy AI đơn giản bằng NavMesh?
- Bake NavMesh cho level, gắn `NavMeshAgent` vào enemy, set destination tới player khi chase, rồi tách state như idle, patrol, chase, attack để logic dễ kiểm soát.

### Làm sao quản lý scene transition cơ bản?
- Thêm scene vào Build Settings, dùng `SceneManager.LoadScene` cho chuyển đơn giản hoặc `LoadSceneAsync` cho loading mượt hơn. Với fade/loading screen, dùng coroutine hoặc transition manager để điều phối.

### Save system cơ bản trong Unity làm thế nào?
- `PlayerPrefs` hợp cho setting nhỏ như volume hoặc tutorial flag. Với data phức tạp hơn, serialize object thành JSON rồi ghi file bằng `System.IO`; không lưu save game quan trọng trực tiếp trong `ScriptableObject`.

---

## Related notes
- [[Definition]]
- [[Senior Interview]]
- [[../MonoBehaviour Lifecycle/MonoBehaviour Lifecycle|MonoBehaviour Lifecycle]]
- [[../Serialization/SerializeField|SerializeField]]
- [[../Asynchronous/Coroutine|Coroutine]]
- [[../Addressables/Addressables|Addressables]]
- [[../Object Pooling/Object Pooling|Object Pooling]]
- [[../Draw Call/Draw Call|Draw Call]]
- [[../Render Pipeline/Render Pipeline|Render Pipeline]]
- [[../Scriptable Object/Scriptable Object|Scriptable Object]]
- [[Summary]]
