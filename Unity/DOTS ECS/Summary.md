---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Entities 1.4.2`
  Đây là package version chính được dùng để đọc các khái niệm ECS trong section này.
  Một số API chi tiết có thể đổi theo package version, nên luôn kiểm tra lại version khi code thật.
- `Unity 6.3`
  Dùng làm mốc cho các phần liên quan tới `Job System` và tài liệu engine-level.
  `Burst` trong Unity `6000.3` được tài liệu manual ghi là package `1.8.28`.

## Core model
- `Entity`
  Là ID nhẹ, không chứa behavior.
  Dùng để gom các component liên quan lại thành một thực thể logic.
- `Component`
  Là data mà system đọc hoặc ghi.
  ECS mạnh khi component giữ vai trò pure data càng rõ càng tốt.
- `System`
  Là nơi chạy logic trên data.
  Behavior không nằm trong entity mà nằm trong các system xử lý hàng loạt entity.

## Memory and performance
- `Archetype`
  Gom entity có cùng tập component type.
  Đây là nền tảng để query theo component chạy nhanh.
- `Chunk`
  Là block memory `16 KiB` chứa entity cùng archetype.
  Data layout này giúp iteration cache-friendly hơn rất nhiều.
- `Structural change`
  Xảy ra khi tạo/hủy entity hoặc thêm/xóa component type.
  Đây là chỗ đắt đỏ cần hạn chế trong hot path.

## Authoring flow
- `Baking`
  Chuyển authoring data từ GameObject sang runtime ECS data trong Editor.
  Là cây cầu giữa workflow authoring linh hoạt và runtime data tối ưu.
- `SubScene`
  Là điểm rất hay đi cùng baking trong workflow ECS.
  Mở subscene thì có thể live/incremental bake, đóng subscene thì full bake nền.

## Execution model
- `World`
  Là container của entity và system.
  Entity chỉ unique bên trong world của nó.
- `Job System`
  Dùng worker threads để xử lý việc song song trên nhiều CPU core.
  Rất hợp với ECS khi số entity lớn và data có thể xử lý theo batch.
- `Burst`
  Là compiler dịch IL/.NET sang native code tối ưu bằng `LLVM`.
  Trong thực tế DOTS thường mạnh nhất khi ECS, Job System, và Burst đi cùng nhau.

## Burst specifics
- `[BurstCompile]`
  Là attribute phổ biến nhất để yêu cầu Burst compile job, method, hoặc system callback tương thích.
  Đây thường là điểm bắt đầu rõ nhất khi bật Burst cho code ECS.
- `JIT và AOT`
  Trong Editor Play Mode, Burst thường compile kiểu `JIT`.
  Trong Player build, Burst compile `AOT` thành native code được ship cùng game.
- `HPC#`
  Burst không hỗ trợ toàn bộ C# mà dùng một high-performance subset.
  Vì vậy code có managed object hoặc construct không tương thích sẽ không tận dụng Burst như mong muốn.
- `First-use behavior`
  Theo tài liệu Burst `1.8`, lần đầu dùng trong Editor Burst thường compile bất đồng bộ.
  Nếu đang profile, nên warmup trước để tránh số đo bị lệch vì compile cost.

## Decision rules
- `Need many similar objects`
  Cân nhắc ECS.
  Đặc biệt đúng khi object có data shape rõ và behavior có thể xử lý theo batch.
- `Need familiar workflow and low complexity`
  Ưu tiên `GameObject` và `MonoBehaviour`.
  ECS có learning cost và pipeline phức tạp hơn.
- `Need authoring in Editor but runtime optimized`
  Dùng baking.
  Đây là workflow chuẩn để đi từ authoring object sang ECS runtime data.
- `Need parallel CPU work`
  Dùng `Job System`, thường kết hợp `Burst`.
  Không nhầm bài toán này với async flow kiểu `Task` hay `Coroutine`.
- `Need maximum DOTS CPU throughput`
  Kết hợp data layout tốt, job song song, và `Burst`.
  Burst chỉ phát huy mạnh khi phần trước đó đã được tổ chức hợp lý.

## Common traps
- `Entity = GameObject`
  Sai. Entity chỉ là ID nhẹ, không phải object behavior-rich như `GameObject`.
  Nếu tư duy sai ở đây thì model ECS sẽ rất khó hiểu.
- `Component = class có method`
  Sai về tinh thần ECS.
  Component nên nghiêng về data, còn logic để system xử lý.
- `Mọi bài toán đều nên dùng ECS`
  Sai.
  ECS rất mạnh cho scale và data-oriented flow, nhưng không phải lúc nào cũng đáng độ phức tạp.
- `Job System = async/await`
  Sai.
  Job System để song song CPU-bound work, còn `async/await` giải quyết flow chờ và continuation.
- `Có Job System là đủ, không cần Burst`
  Không hẳn.
  Job System giải quyết song song hóa, còn Burst là lớp compile tối ưu rất quan trọng cho CPU hot path.
- `Burst làm nhanh mọi C# code`
  Sai.
  Burst chỉ compile phần code tương thích với HPC# và không thay thế việc thiết kế data layout đúng.

## Interview questions and answers
### Entity khác `GameObject` ở điểm cốt lõi nào?
- Entity là ID nhẹ không chứa behavior, còn `GameObject` là object-oriented container có component theo workflow truyền thống của Unity.

### Vì sao archetype và chunk quan trọng trong ECS?
- Vì chúng quyết định cách data được nhóm và đặt trong memory, từ đó ảnh hưởng trực tiếp tới cache locality và tốc độ query.

### Baking giải quyết vấn đề gì?
- Nó cho phép authoring bằng `GameObject` trong Editor nhưng runtime dùng data ECS tối ưu hơn.

### Khi nào nên dùng `ISystem` hơn `SystemBase`?
- Khi bạn muốn unmanaged system với callback có thể Burst compile và không cần model managed như `SystemBase`.

### Job System có thay thế `Coroutine` hay `async/await` không?
- Không. Job System chủ yếu để song song hóa CPU-bound work, còn `Coroutine` và `async/await` xử lý sequencing và chờ đợi.

### Burst Compiler giải quyết phần nào trong stack DOTS?
- Nó biên dịch phần code tương thích sang native code tối ưu, còn ECS tổ chức data và Job System tổ chức việc chạy song song trên CPU.

## Related notes
- [[DOTS ECS]]
- [[Definition]]
- [[Entity]]
- [[Components]]
- [[Systems]]
- [[World]]
- [[Archetypes and Chunks]]
- [[Baking]]
- [[Burst Compiler]]
- [[Job System]]
- [[../Unity|Unity]]
