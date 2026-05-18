---
note_type: guide
tags:
  - vault
---

# Vault Format System

Vault này dùng format linh hoạt theo loại note, không ép mọi feature note phải có cùng một bộ heading.
Mục tiêu là mỗi note đủ ngắn để scan nhanh, đủ đặc để nhớ đúng, và đủ liên kết để mở rộng khi cần.

## Writing principles
- Front-load knowledge: câu đầu tiên phải nói ý chính, không mở đầu bằng bối cảnh dài.
- Viết bullet ngắn, ưu tiên một ý mỗi bullet.
- Dùng keyword ở đầu bullet khi có thể, ví dụ `GC`, `Cull Back`, `mipmap`, `shader_feature`.
- Dùng active voice và động từ cụ thể.
- Tránh filler như `rất`, `nhiều`, `một số`, `về cơ bản` nếu không thêm ý nghĩa.
- Dùng heading để scan: người đọc phải hiểu note nói gì chỉ bằng heading và keyword.
- Mỗi feature note nên bám một concept chính. Nếu note bắt đầu nói `and also`, tách thành note mới.
- Review bằng recall: summary note nên có câu hỏi tự kiểm tra, không chỉ liệt kê lại nội dung.

## Required anchors
- `field` note
  Dùng để mô tả phạm vi của field folder và link tới các section chính.
- `section` note
  Dùng `## Scope`, `## Note map`, `## Related notes`.
- `definition` note
  Dùng `## Definition`, `## Related notes`.
- `feature` note
  Dùng `## Core idea`, `## Key points`, `## Decision rules`, tùy chọn `## Example`, và `## Related notes`.
  Nếu note thật sự cần cấu trúc khác, chọn heading từ section bank bên dưới.
- `summary` note
  Dùng để review: `## Core keywords`, `## Decision rules`, `## Common traps`, câu hỏi review, và `## Related notes`.

## Section bank
- `## Core idea`
  Một câu hoặc vài bullet nói note này giải quyết ý gì.
- `## Definition`
  Định nghĩa ngắn, ranh giới khái niệm, hoặc bản chất của topic.
- `## Key points`
  Các fact quan trọng nhất để hiểu topic.
  Gộp định nghĩa, cơ chế, API, hoặc setting khi tách ra sẽ quá dài.
- `## Context`
  Dùng khi cần biết topic nằm trong hệ thống nào hoặc liên quan tới vấn đề nào.
- `## Mental model`
  Dùng cho khái niệm khó, cần hình dung bằng mô hình đơn giản.
- `## Mechanics`
  Cách hoạt động bên trong, flow, lifecycle, hoặc data path.
- `## Workflow`
  Các bước thao tác hoặc quy trình sử dụng.
- `## API surface`
  Class, method, attribute, property, setting, hoặc directive quan trọng.
- `## Parameters`
  Ý nghĩa tham số, option, flag, hoặc setting.
- `## Data model`
  Dùng khi topic xoay quanh state, memory layout, serialization, hoặc resource ownership.
- `## Use cases`
  Vì sao dùng, vấn đề nào được giải quyết.
- `## Use when`
  Điều kiện nên dùng.
- `## Avoid when`
  Điều kiện không nên dùng.
- `## Decision rules`
  Rule chọn giữa nhiều option.
- `## Tradeoffs`
  Giới hạn, cost, performance risk, hoặc maintenance risk.
- `## Common traps`
  Lỗi hiểu sai hoặc lỗi implementation thường gặp.
- `## Performance notes`
  Cost, allocation, GPU/CPU impact, memory impact, hoặc profiling note.
- `## Example`
  Code, pseudo-code, config, workflow mẫu, hoặc mini case study.
- `## Review questions`
  Câu hỏi tự kiểm tra hoặc interview.
- `## Related notes`
  Wikilink tới note liên quan.

## Recommended shapes
### Compact feature note
```md
## Core idea
## Key points
## Decision rules
## Example
## Related notes
```

### Concept note
```md
## Core idea
## Definition
## Mental model
## Use cases
## Tradeoffs
## Related notes
```

### API note
```md
## Core idea
## Definition
## API surface
## Parameters
## Mechanics
## Example
## Common traps
## Related notes
```

### Pattern note
```md
## Core idea
## Problem
## Structure
## Workflow
## Use when
## Avoid when
## Tradeoffs
## Example
## Related notes
```

### Settings note
```md
## Core idea
## Definition
## Important settings
## Decision rules
## Common traps
## Example
## Related notes
```

### Performance note
```md
## Core idea
## Definition
## Mechanics
## Performance notes
## Decision rules
## Common traps
## Related notes
```

## Rules
- Không tạo heading nếu section đó không giúp review.
- Không giữ section rỗng chỉ để giống template.
- Feature note mặc định dùng compact shape. Chỉ tách thêm heading khi `Key points` hoặc `Decision rules` quá dài.
- Mục tiêu mềm: feature note khoảng 200-500 từ, summary note khoảng 250-700 từ.
- Nếu ví dụ code dài hơn phần giải thích, rút ví dụ xuống path chính hoặc pseudo-code.
- Đổi tên heading theo vai trò thật của nội dung, ví dụ API note nên có `API surface`, settings note nên có `Important settings`.
- Feature note không bắt buộc có code. Nếu ví dụ là workflow hoặc config, vẫn dùng `## Example`.
- Summary note được phép có section domain-specific như `## Rendering paths`, `## Lifecycle`, hoặc `## Architecture patterns`.
- Heading nên nhất quán về ý nghĩa, không nhất thiết giống nhau tuyệt đối giữa mọi note.

## Source influence
- Technical writing: active voice, short sentences, concrete words, and no filler.
- Web writing: important information first, scannable headings, lists for complicated content.
- Cornell notes: cues and review questions improve recall.
- Atomic notes: one focused idea per note, complete enough to stand alone, linked to related notes.
