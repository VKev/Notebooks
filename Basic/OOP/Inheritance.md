---
aliases:
  - Class Inheritance
note_type: feature
tags:
  - basic
---

## One-line
- `Inheritance` là cơ chế cho phép class con nhận lại dữ liệu hoặc hành vi từ class cha và mở rộng chúng khi cần.

## What is it
- `Inheritance` mô tả quan hệ `is-a` giữa class con và class cha.
- Class con có thể dùng lại method hoặc field của class cha, rồi bổ sung hoặc override hành vi.

## How it works
- Bạn tạo một base class chứa phần chung.
- Class con kế thừa từ base class và có thể override method `virtual` hoặc `abstract`.
- Code chung nằm ở class cha, còn phần đặc thù nằm ở class con.

## Why use it
- Giảm lặp code khi nhiều class có chung behavior nền.
- Giúp biểu diễn quan hệ cha con rõ ràng khi domain thực sự có dạng phân cấp.

## When to use it
- Dùng khi có quan hệ cha con ổn định và hợp lý theo domain.
- Dùng khi class con thật sự là phiên bản chuyên biệt của class cha.

## When to not use it
- Không nên dùng inheritance chỉ để reuse code nhanh.
- Không nên tạo cây kế thừa sâu nếu composition hoặc interface làm rõ intent hơn.

## Limitations
- Inheritance tạo coupling mạnh giữa class con và class cha.
- Base class đổi sai cách có thể ảnh hưởng hàng loạt class con.
- Cây kế thừa sâu thường khó hiểu và khó refactor.

---

## Example code
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

---

## Related notes
- [[Definition]]
- [[Polymorphism]]
- [[Summary]]
