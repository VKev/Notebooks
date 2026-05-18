---
aliases:
  - OCP
note_type: feature
tags:
  - basic
---

## Core idea
- `Open Closed Principle` khuyến khích mở rộng hành vi mà không phải sửa quá nhiều vào code ổn định hiện có.

## Key points
- Ý chính của `OCP` là module nên “open for extension, closed for modification”.
- Khi cần thêm behavior mới, bạn ưu tiên thêm implementation mới thay vì sửa chuỗi `if-else` cũ liên tục.
- Bạn đặt variation point ở abstraction như interface hoặc base class.
- Mỗi behavior mới được thêm bằng class mới implement contract đó.
- Code sử dụng abstraction không cần biết có bao nhiêu implementation phía sau.

## Decision rules
- Khi behavior có khả năng tăng dần theo loại, rule, hoặc kênh xử lý khác nhau.
- Khi bạn muốn thêm feature mới mà ít chạm vào code cũ.
- Tránh tạo abstraction từ đầu nếu chưa có dấu hiệu biến thiên thực sự.
- Tránh thêm nhiều layer mở rộng cho bài toán nhỏ và ổn định.
- Không phải mọi chỗ đều cần “closed for modification”; có nơi sửa trực tiếp lại đơn giản và đúng hơn.
- Giảm rủi ro làm hỏng code cũ khi thêm tính năng mới.
- Hỗ trợ mở rộng hệ thống theo hướng an toàn hơn.
- Giúp pattern như `Strategy`, `Decorator`, `Factory` phát huy tác dụng.

## Example
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

## Related notes
- [[Definition]]
- [[Dependency Inversion Principle]]
- [[Summary]]
