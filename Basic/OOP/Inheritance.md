---
aliases:
  - Class Inheritance
note_type: feature
tags:
  - basic
---

## Core idea
- `Inheritance` là cơ chế cho phép class con nhận lại dữ liệu hoặc hành vi từ class cha và mở rộng chúng khi cần.

## Key points
- `Inheritance` mô tả quan hệ `is-a` giữa class con và class cha.
- Class con có thể dùng lại method hoặc field của class cha, rồi bổ sung hoặc override hành vi.
- Bạn tạo một base class chứa phần chung.
- Class con kế thừa từ base class và có thể override method `virtual` hoặc `abstract`.
- Code chung nằm ở class cha, còn phần đặc thù nằm ở class con.

## Decision rules
- Khi có quan hệ cha con ổn định và hợp lý theo domain.
- Khi class con thật sự là phiên bản chuyên biệt của class cha.
- Tránh dùng inheritance chỉ để reuse code nhanh.
- Tránh tạo cây kế thừa sâu nếu composition hoặc interface làm rõ intent hơn.
- Giảm lặp code khi nhiều class có chung behavior nền.
- Giúp biểu diễn quan hệ cha con rõ ràng khi domain thực sự có dạng phân cấp.
- Inheritance tạo coupling mạnh giữa class con và class cha.
- Base class đổi sai cách có thể ảnh hưởng hàng loạt class con.

## Example
```csharp
public class Enemy
{
    public virtual int GetDamage()
    {
        return 10;
    }
}
public class BossEnemy : Enemy
{
    public override int GetDamage()
    {
        return 30;
    }
}
```

## Related notes
- [[Definition]]
- [[Polymorphism]]
- [[Summary]]
