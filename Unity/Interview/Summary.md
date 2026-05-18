---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Core keywords
- `Junior Interview`
  Nắm chắc workflow Unity, component model, lifecycle, prefab, serialization, coroutine, physics, UI, và debugging cơ bản.
  Mục tiêu là làm task nhỏ đúng yêu cầu, ít phá cấu trúc project, và biết sửa bug có reproduction.
- `Senior Interview`
  Nắm trade-off architecture, lifecycle ownership, profiling, memory, asset loading, rendering cost, async cleanup, build/platform, và tooling.
  Mục tiêu là thiết kế bền, tối ưu có bằng chứng, giảm rủi ro production, và giúp team làm đúng hơn.
- `Level signal`
  Junior trả lời đúng khái niệm và dùng API đúng chỗ.
  Senior giải thích được nguyên nhân, trade-off, constraint, failure mode, và cách kiểm chứng.
- `Question bank`
  Câu hỏi được đặt trực tiếp trong `Junior Interview` và `Senior Interview`.
  Không dùng note trung gian để tránh phân tán khi ôn tập.

## Junior checklist
- `Scene and Prefab`
  Biết tạo scene, prefab, prefab variant, và xử lý override cơ bản.
  Không phá reference hoặc apply override ngoài ý muốn.
- `MonoBehaviour Lifecycle`
  Hiểu `Awake`, `OnEnable`, `Start`, `Update`, `FixedUpdate`, `OnDisable`, `OnDestroy`.
  Biết cleanup event khi lifecycle kết thúc.
- `Inspector and Serialization`
  Dùng `[SerializeField] private` cho config.
  Biết Unity serialize field theo rule riêng.
- `Coroutine and AsyncOperation`
  Biết coroutine không phải thread.
  Biết chờ theo frame, thời gian, hoặc operation.
- `Debugging`
  Biết đọc Console, stack trace, null reference, missing reference.
  Biết tái hiện bug trước khi sửa.

## Senior checklist
- `Architecture`
  Tách responsibility và dependency rõ.
  Không over-engineer khi feature nhỏ, nhưng cũng không để coupling làm project khó mở rộng.
- `Lifecycle and Ownership`
  Rõ ai tạo, ai giữ reference, ai cleanup.
  Chặn bug do async callback, event leak, hoặc destroyed object.
- `Profiling`
  Đo trước khi tối ưu.
  Phân biệt CPU, GPU, memory, loading, GC, và rendering bottleneck.
- `Asset Loading`
  Biết chọn direct reference, `ScriptableObject`, Addressables, hoặc scene loading theo nhu cầu.
  Luôn có release strategy cho asset load động.
- `Team Workflow`
  Tạo guideline, validation, editor tool, hoặc build check khi lỗi lặp lại.
  Review code dựa trên rủi ro thật.

## Interview questions and answers
### Nên luyện câu hỏi Unity theo thứ tự nào?
- Bắt đầu từ junior để chắc lifecycle, prefab, serialization, coroutine, physics, UI, và debug. Sau đó lên middle với performance, Addressables, object pooling, event cleanup. Cuối cùng luyện senior với architecture, profiling, ownership, memory, và production risk.

### Junior Unity developer cần nắm chắc gì nhất?
- Component model, prefab workflow, lifecycle, serialization Inspector, coroutine cơ bản, physics cơ bản, UI cơ bản, và cách debug lỗi thường gặp.

### Senior Unity developer khác junior ở điểm nào?
- Senior không chỉ biết API mà còn biết chọn trade-off, thiết kế ownership rõ, profile trước khi tối ưu, quản lý memory/loading, và giảm rủi ro cho cả team.

### Làm sao đánh giá một người đã lên senior Unity chưa?
- Hỏi cách họ xử lý bug khó, performance issue, asset loading, lifecycle cleanup, và thiết kế feature có thay đổi requirement. Senior phải có reasoning và cách kiểm chứng rõ.

### Junior nên tập trung học gì trước khi học DOTS hoặc render pipeline sâu?
- Nên chắc `MonoBehaviour Lifecycle`, prefab, serialization, coroutine, physics, UI, object pooling, draw call cơ bản, và debugging.

### Senior có cần biết mọi API Unity không?
- Không. Senior cần biết cách tìm đúng tài liệu, hiểu hệ thống cốt lõi, đánh giá trade-off, và xây giải pháp ổn định trong constraint cụ thể.

## Related notes
- [[Interview]]
- [[Definition]]
- [[Junior Interview]]
- [[Senior Interview]]
- [[../MonoBehaviour Lifecycle/MonoBehaviour Lifecycle|MonoBehaviour Lifecycle]]
- [[../Serialization/Serialization|Serialization]]
- [[../Scriptable Object/Scriptable Object|Scriptable Object]]
- [[../Addressables/Addressables|Addressables]]
- [[../Object Pooling/Object Pooling|Object Pooling]]
- [[../Draw Call/Draw Call|Draw Call]]
- [[../Render Pipeline/Render Pipeline|Render Pipeline]]
