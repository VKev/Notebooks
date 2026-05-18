---
aliases:
  - Senior Unity Interview
  - Unity Senior Checklist
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Senior Interview` là nhóm kiến thức Unity dùng để trả lời phỏng vấn senior, thiết kế hệ thống bền, tối ưu có bằng chứng, xử lý rủi ro production, và hướng dẫn người khác làm đúng.

## What is it
- Senior Unity developer không chỉ biết API, mà phải hiểu trade-off giữa kiến trúc, performance, workflow của team, khả năng debug, và chi phí bảo trì.
- Senior cần biết khi nào dùng `MonoBehaviour`, khi nào dùng `ScriptableObject`, khi nào tách pure C# service, khi nào dùng Addressables, khi nào cần profiling trước khi tối ưu.
- Senior phải giải thích được quyết định kỹ thuật bằng constraint cụ thể: platform, memory budget, content size, team workflow, build size, load time, frame budget, và testability.

## Core topics
- `Architecture`
  Tách gameplay rule khỏi Unity glue khi có thể.
  Dùng interface, event, state machine, command, hoặc dependency injection vừa đủ để giảm coupling thật, không tạo abstraction giả.
- `Lifecycle ownership`
  Xác định rõ object nào tạo, object nào giữ reference, object nào cleanup.
  Bug production thường đến từ lifecycle không rõ: event leak, object destroyed nhưng reference còn sống, async callback trả về sau khi owner đã bị destroy.
- `Asset and memory`
  Hiểu direct reference, scene reference, Resources, Addressables, AssetBundle, và `ScriptableObject` khác nhau ở loading và memory.
  Có strategy load/unload rõ ràng cho content lớn.
- `Performance profiling`
  Tối ưu dựa trên Profiler, Memory Profiler, Frame Debugger, RenderDoc hoặc platform tool khi cần.
  Không tối ưu theo cảm giác nếu chưa xác định bottleneck CPU, GPU, memory, IO, hoặc loading.
- `Rendering cost`
  Hiểu draw call, batching, SRP Batcher, GPU instancing, overdraw, shader variant, texture memory, shadow cost, post-processing.
  Biết tối ưu theo target platform thay vì một checklist chung.
- `Async and loading`
  Hiểu coroutine, `AsyncOperation`, Addressables async handle, `async/await`, cancellation, error path, và cleanup khi object bị destroy.
  Senior phải tránh callback ghi vào object đã hết lifecycle.
- `Data design`
  Dùng `ScriptableObject` cho shared config hoặc data-driven workflow.
  Không dùng nó như save game store cuối cùng trong build.
- `Build and platform`
  Biết build setting, scripting backend, managed stripping, asset compression, quality level, resolution, input, platform permission, và device profiling.
  Không coi Editor performance là đại diện hoàn toàn cho build.
- `Tooling`
  Tạo editor tool, validation, import rule, build check, hoặc debug view khi nó giảm lỗi lặp lại cho team.
  Tool tốt giúp junior tránh sai thay vì chỉ nhắc bằng tài liệu.

## Practical expectations
- Thiết kế feature có thể mở rộng vừa đủ và vẫn dễ đọc.
- Đưa ra trade-off rõ ràng giữa tốc độ làm, độ bền, performance, và workflow content.
- Debug bug khó bằng reproduction, logging có chủ đích, profiler capture, và giả thuyết kiểm chứng được.
- Review code tập trung vào lifecycle, ownership, allocation, coupling, serialization, và edge case.
- Biết giảm scope refactor để không phá feature đang chạy.
- Biết viết guideline hoặc tool nhỏ để cả team làm đúng hơn.

## Senior decision rules
- `Feature nhỏ, lifecycle đơn giản`
  Dùng `MonoBehaviour` trực tiếp và giữ code rõ.
  Không cần framework hóa sớm.
- `Logic cần test hoặc reuse ngoài scene`
  Tách pure C# class/service ra khỏi Unity component.
  `MonoBehaviour` chỉ làm wiring với scene, input, hoặc view.
- `Data config dùng chung nhiều prefab hoặc scene`
  Dùng `ScriptableObject`.
  Nếu là runtime player progress, dùng save system riêng.
- `Asset lớn hoặc load theo ngữ cảnh`
  Dùng Addressables hoặc loading layer rõ ràng.
  Luôn định nghĩa owner và thời điểm release.
- `Performance giảm`
  Đo trước bằng profiler.
  Xác định bottleneck rồi mới chọn tối ưu.
- `Nhiều object cần update mỗi frame`
  Cân nhắc update manager, batching logic, hoặc data-oriented approach.
  Không để hàng ngàn `Update` nhỏ chạy rải rác nếu profiler đã chỉ ra overhead.

## Common senior traps
- Over-engineering architecture cho feature nhỏ.
- Dùng singleton/service locator quá nhiều làm dependency ẩn.
- Tối ưu rendering nhưng bottleneck thật là script hoặc GC.
- Tin kết quả trong Editor thay vì profile trên device/build target.
- Dùng Addressables nhưng không quản lý handle và release rõ ràng.
- Tạo abstraction khó hiểu hơn vấn đề ban đầu.
- Refactor lớn mà không có test, capture, hoặc rollback path.

## Interview signals
- Trả lời tốt khi nói được cách xác định bottleneck trước khi tối ưu.
- Trả lời tốt khi phân biệt config data, runtime state, save data, và asset reference.
- Trả lời tốt khi có chiến lược cleanup event/async/loading theo lifecycle.
- Trả lời tốt khi không chọn pattern theo tên, mà chọn theo áp lực thiết kế.
- Trả lời tốt khi biết trade-off giữa direct reference và Addressables.

---

## Interview questions and answers
### Direct reference và Addressables khác nhau thế nào?
- Direct reference đơn giản nhưng có thể kéo asset vào memory theo reference chain. Addressables cho phép load async theo địa chỉ và release có chủ đích, nhưng cần quản lý handle/lifecycle chặt hơn.

### Addressables và `Resources.Load` khác nhau thế nào?
- `Resources.Load` chỉ load asset nằm trong folder `Resources` bằng path string và dễ làm asset bị đóng gói rộng. Addressables quản lý asset bằng address/label, hỗ trợ async load, dependency tracking, remote content, và release rõ hơn.

### Làm sao tránh khựng khi load scene hoặc asset lớn?
- Dùng async loading, preload dependency, chia nhỏ initialization qua nhiều frame, hiển thị loading UI, và chỉ activate scene khi đã sẵn sàng. Với Addressables, cần quản lý handle và release sau khi không dùng.

### Vì sao cần unsubscribe event?
- Nếu subscriber vẫn nằm trong invocation list của publisher, nó có thể bị giữ reference hoặc nhận callback sau khi object đã disable/destroy. Thường unsubscribe trong `OnDisable`.

### `ScriptableObject` có persist runtime change trong build không?
- Không nên xem nó như nơi lưu progress cuối cùng. Trong build, thay đổi runtime không persist qua lần chạy sau. Muốn lưu player progress cần save system riêng như JSON, binary, cloud save, hoặc database.

### 10,000 `MonoBehaviour.Update` riêng lẻ và một update manager khác nhau thế nào?
- Nhiều `Update` nhỏ tạo overhead dispatch và khó kiểm soát thứ tự/tần suất. Update manager có thể gom logic, tắt bật rõ hơn, và chỉ update object thật sự cần, nhưng không nên thêm abstraction nếu profiler chưa chỉ ra vấn đề.

### Khi nào dùng `async/await` thay coroutine trong Unity?
- Dùng khi làm việc với API async hoặc flow bất đồng bộ cần return result/exception rõ hơn. Vẫn phải chú ý main thread, cancellation, và lifecycle object bị destroy.

### Làm sao debug một bug chỉ xảy ra thỉnh thoảng?
- Tạo reproduction nhỏ nhất có thể, thêm log có context, kiểm tra lifecycle/order, capture profiler nếu liên quan performance, rồi kiểm chứng từng giả thuyết thay vì sửa ngẫu nhiên.

### Khi game bị tụt FPS, senior nên làm gì trước?
- Tái hiện vấn đề, profile trên build hoặc device gần target, xác định bottleneck CPU/GPU/memory/IO, rồi mới chọn hướng tối ưu. Không sửa theo cảm giác.

### Làm sao phân biệt CPU bottleneck và GPU bottleneck?
- Xem profiler/frame timing. Nếu main thread hoặc render thread tốn nhiều thời gian, nghi CPU. Nếu GPU frame time cao, overdraw, shader, shadow, post-processing, hoặc fill-rate có thể là nguyên nhân.

### Khi nào tách logic khỏi `MonoBehaviour`?
- Khi logic cần test, reuse, hoặc không phụ thuộc trực tiếp vào scene lifecycle. Khi đó `MonoBehaviour` nên giữ vai trò adapter/wiring.

### Làm sao thiết kế ownership cho async loading?
- Xác định object nào request load, object nào giữ handle, lúc nào cancel/release, và callback phải kiểm tra owner còn valid không trước khi ghi vào state.

### Khi nào dùng `ScriptableObject` thay vì singleton?
- Khi cần data/config share được, nhiều instance độc lập, Inspector workflow, và giảm coupling. Singleton chỉ hợp khi domain thật sự cần một instance global có lifecycle rõ.

### Khi nào Addressables không đáng dùng?
- Khi project nhỏ, asset ít, không cần remote content, không cần load/unload theo ngữ cảnh, hoặc overhead quản lý Addressables lớn hơn lợi ích.

### Direct reference và Addressables khác nhau thế nào về memory?
- Direct reference có xu hướng kéo asset vào cùng scene hoặc object reference chain. Addressables cho phép trì hoãn load và chủ động release, nhưng yêu cầu quản lý async handle và lifecycle chặt hơn.

### Senior review Unity code thường nhìn gì?
- Lifecycle ownership, cleanup event, allocation trong hot path, reference tới scene/asset, serialization rule, async error path, và coupling giữa gameplay systems.

### Làm sao giảm rủi ro khi refactor hệ thống gameplay lớn?
- Chia nhỏ scope, giữ behavior có thể kiểm chứng, thêm test hoặc debug capture nếu có, migrate từng phần, và có rollback path. Không refactor rộng khi chưa hiểu dependency.

### Khi nào nên tạo editor tool?
- Khi lỗi setup lặp lại nhiều lần, designer cần workflow ổn định, hoặc validation thủ công tốn thời gian. Tool tốt biến rule thành kiểm tra tự động.

### Khi nào nên dùng Job System hoặc Burst?
- Dùng khi có CPU-bound workload lớn, data độc lập, có thể chia nhỏ và chạy song song như pathfinding batch, simulation, transform/math processing. Không dùng nếu logic phụ thuộc nhiều Unity object managed hoặc cần gọi Unity API main thread liên tục.

### Job System cần chú ý gì khi giao tiếp với main thread?
- Job không nên truy cập trực tiếp hầu hết Unity API hoặc managed object. Dữ liệu nên đi qua `NativeArray`, `NativeList`, hoặc container phù hợp, và main thread chịu trách nhiệm schedule, complete, apply kết quả, dispose memory đúng lúc.

### Forward rendering và deferred rendering khác nhau thế nào?
- Forward render tính lighting trong lúc vẽ object, thường đơn giản và hợp nhiều nền tảng. Deferred tách geometry pass và lighting pass, hợp nhiều dynamic lights hơn nhưng tốn memory bandwidth và có constraint về transparency/MSAA/platform.

### Làm sao quản lý memory cho scene lớn?
- Kiểm soát direct reference, dùng Addressables hoặc scene streaming khi cần, release asset đúng owner, giảm texture/audio memory, tránh duplicate material, và đo bằng Memory Profiler.

### AssetBundle hoặc Addressables content nên chọn compression thế nào?
- Không nén load nhanh nhưng file lớn. LZMA nén tốt nhưng thường phải giải nén nguyên bundle nên không hợp runtime random load. LZ4 cân bằng hơn cho runtime vì chunk-based và thường hợp live game/content delivery hơn.

### Làm sao thiết kế hot update hoặc content update an toàn?
- Tách code và content boundary rõ, version catalog/manifest, kiểm tra dependency, hỗ trợ rollback, log lỗi tải, và có chiến lược fallback khi remote content hỏng hoặc mạng yếu.

### Unity project nên có CI kiểm tra gì?
- Build theo target platform, chạy test, validate asset rule, kiểm tra missing reference, kiểm tra addressable/build content, tạo artifact, và ghi version/build metadata để trace lỗi production.

### Unity có thể test tự động những gì?
- Có thể test pure C# logic bằng edit mode tests, test MonoBehaviour/scene flow bằng play mode tests, và thêm validation test cho prefab, addressables, asset import setting, hoặc performance baseline.

### State sync và frame sync khác nhau thế nào trong game networking?
- State sync gửi trạng thái authoritative hoặc snapshot để client nội suy/dự đoán. Frame sync gửi input/command theo tick và yêu cầu simulation deterministic hơn. Chọn theo game type, độ trễ, cheat risk, và chi phí đồng bộ.

### Làm sao chọn giữa OOP, event-driven, và data-oriented approach?
- Chọn theo pressure thật. OOP hợp model rõ và team dễ đọc. Event-driven hợp decoupling giữa systems. Data-oriented hợp số lượng entity lớn hoặc performance cần tối ưu theo cache/job.

### Một senior trả lời tốt về performance khác junior thế nào?
- Junior thường nêu checklist tối ưu. Senior bắt đầu từ đo đạc, xác định bottleneck, nêu trade-off, kiểm chứng kết quả, và cân nhắc tác động tới workflow/team.

### Làm sao quản lý AssetBundle cho project lớn?
- Chia bundle theo cách asset được load cùng nhau, kiểm soát dependency để tránh duplicate, chọn compression theo use case, load async khi cần, unload bundle/asset đúng thời điểm, và theo dõi manifest/version để hỗ trợ content update.

### Tạo và dùng custom shader trong Unity như thế nào?
- Tạo shader asset, định nghĩa property và pass/subshader theo pipeline đang dùng, tạo material dùng shader đó, rồi gán material cho renderer. Với production, cần kiểm soát variant, batching compatibility, target platform, và debug bằng Frame Debugger/RenderDoc khi cần.

### Realtime Global Illumination ảnh hưởng performance như thế nào?
- Realtime GI tăng chi phí CPU/GPU và memory vì phải cập nhật indirect lighting. Senior cần cân bằng lightmap resolution, bounce, shadow, light probe density, và cân nhắc baked lighting nếu scene đủ static.

### Lightmapping dùng để giải quyết gì?
- Lightmapping bake ánh sáng static thành texture để giảm chi phí runtime. Nó hợp cho môi trường ít thay đổi, kết hợp light probes để dynamic object nhận lighting gần đúng.

### Chọn Built-in, URP, hay HDRP như thế nào?
- Chọn theo target platform và visual requirement. Built-in hợp project cũ hoặc đơn giản, URP hợp đa nền tảng/mobile/VR với performance tốt, HDRP hợp PC/console high-end cần visual fidelity cao.

### Tối ưu Unity game cho mobile nên bắt đầu từ đâu?
- Profile trên device thật, giảm overdraw, giảm realtime shadow/light, dùng texture compression phù hợp, giảm polygon/material, pool object lặp lại, tránh allocation trong hot path, và kiểm soát thermal/battery.

### Dùng Unity Profiler để xử lý performance issue như thế nào?
- Capture trên build hoặc device gần target, tìm frame spike, drill down vào Rendering/Scripts/Physics/Animation/GC, sửa một thay đổi nhỏ, rồi profile lại để xác nhận. Không kết luận chỉ từ cảm giác trong Editor.

### Custom Editor tool nên được tạo theo bước nào?
- Tạo script trong folder `Editor`, kế thừa `Editor` hoặc `EditorWindow`, dựng UI bằng IMGUI/UI Toolkit, thao tác asset/scene qua API editor, và thêm validation để tránh phá prefab hoặc scene data.

### Các script giao tiếp với nhau nên dùng cách nào?
- Reference trực tiếp hợp khi dependency rõ và local. Interface hợp contract, event hợp broadcast, `ScriptableObject` hợp shared data/event channel. Tránh `SendMessage` hoặc global singleton nếu chỉ để né thiết kế dependency.

### ECS gồm những phần nào?
- ECS gồm `Entity` là ID nhẹ, `Component` là data thuần, và `System` là logic xử lý tập entity có component phù hợp. Cách này hợp workload lớn, data-oriented, cần cache-friendly và parallel-friendly hơn OOP truyền thống.

### Thiết kế multiplayer networking trong Unity cần quan tâm gì?
- Cần chọn authority model, transport, state sync hoặc frame sync, tick rate, interpolation/prediction, reconciliation, lag compensation, server validation, reconnect, và chống cheat ở server side.

### Dùng Photon cho real-time multiplayer cần flow cơ bản nào?
- Connect tới Photon, join/create room, instantiate networked player, đồng bộ transform/state qua `PhotonView` hoặc custom serialization, chỉ xử lý input cho local owner, và xử lý disconnect/player leave rõ ràng.

---

## Related notes
- [[Definition]]
- [[Junior Interview]]
- [[../Scriptable Object/Scriptable Object|Scriptable Object]]
- [[../Addressables/Addressables|Addressables]]
- [[../Object Pooling/Object Pooling|Object Pooling]]
- [[../Draw Call/Draw Call|Draw Call]]
- [[../Render Pipeline/Render Pipeline|Render Pipeline]]
- [[../DOTS ECS/Job System|Job System]]
- [[../DOTS ECS/DOTS ECS|DOTS ECS]]
- [[../Asynchronous/AsyncAwait|AsyncAwait]]
- [[Summary]]
