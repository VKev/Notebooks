---
aliases:
  - OCP
note_type: feature
tags:
  - basic
---

## One-line
- `Open Closed Principle` khuyến khích mở rộng hành vi mà không phải sửa quá nhiều vào code ổn định hiện có.

## What is it
- Ý chính của `OCP` là module nên “open for extension, closed for modification”.
- Khi cần thêm behavior mới, bạn ưu tiên thêm implementation mới thay vì sửa chuỗi `if-else` cũ liên tục.

## How it works
- Bạn đặt variation point ở abstraction như interface hoặc base class.
- Mỗi behavior mới được thêm bằng class mới implement contract đó.
- Code sử dụng abstraction không cần biết có bao nhiêu implementation phía sau.

## Why use it
- Giảm rủi ro làm hỏng code cũ khi thêm tính năng mới.
- Hỗ trợ mở rộng hệ thống theo hướng an toàn hơn.
- Giúp pattern như `Strategy`, `Decorator`, `Factory` phát huy tác dụng.

## When to use it
- Dùng khi behavior có khả năng tăng dần theo loại, rule, hoặc kênh xử lý khác nhau.
- Dùng khi bạn muốn thêm feature mới mà ít chạm vào code cũ.

## When to not use it
- Không nên tạo abstraction từ đầu nếu chưa có dấu hiệu biến thiên thực sự.
- Không nên thêm nhiều layer mở rộng cho bài toán rất nhỏ và ổn định.

## Limitations
- Tạo abstraction quá sớm có thể gây over-engineering.
- Không phải mọi chỗ đều cần “closed for modification”; có nơi sửa trực tiếp lại đơn giản và đúng hơn.

---

## Example code
```csharp
public interface IDiscountStrategy
{
    decimal Apply(decimal price);
}

public class NoDiscount : IDiscountStrategy
{
    public decimal Apply(decimal price) => price;
}

public class VipDiscount : IDiscountStrategy
{
    public decimal Apply(decimal price) => price * 0.8m;
}

public class CheckoutService
{
    public decimal Checkout(decimal price, IDiscountStrategy discountStrategy)
    {
        return discountStrategy.Apply(price);
    }
}
```

---

## Related notes
- [[Definition]]
- [[Dependency Inversion Principle]]
- [[Summary]]
