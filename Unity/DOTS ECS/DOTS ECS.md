---
note_type: structure
tags:
  - unity
---

# DOTS ECS

## Scope
- Nhóm note này chứa các concept về Unity DOTS, ECS architecture, baking workflow, và data-oriented runtime model.
- Father note này đóng vai trò `structure note` cho toàn bộ folder.

## Notes
- [[Definition]]: định nghĩa DOTS/ECS và mental model data-oriented của Unity.
- [[Comparison]]: khi nào nên dùng DOTS/ECS thay vì GameObject hoặc MonoBehaviour workflow truyền thống.
- [[Entity]]: entity là identifier nhẹ, không mang behavior.
- [[Components]]: component là data unit của ECS và các kiểu component phổ biến.
- [[Systems]]: system là nơi logic query và xử lý entities/components.
- [[World]]: world là container cho entities và systems của một runtime context.
- [[Archetypes and Chunks]]: cách ECS gom entity theo component set và layout memory.
- [[Baking]]: quy trình đổi authoring GameObject data sang runtime ECS data.
- [[Job System]]: compute scheduler native của Unity thường đi cùng DOTS.

## Related structure notes
- [[Unity]]: quy luật note cho Unity field.
- [[Bất đồng bộ]]: các note async, task, awaitable, thread, và scheduler.
