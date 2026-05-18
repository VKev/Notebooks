---
aliases:
  - Strategy Pattern
note_type: feature
tags:
  - basic
---

## Core idea
- `Strategy` đóng gói nhiều thuật toán hoặc rule khác nhau sau cùng một interface để có thể thay đổi linh hoạt.

## Key points
- `Strategy` là behavioral pattern.
- Nó tách phần biến thiên của thuật toán ra khỏi nơi sử dụng.
- Bạn định nghĩa contract chung như `IDiscountStrategy`.
- Mỗi strategy cài đặt một cách xử lý riêng.
- Context nhận strategy từ bên ngoài và dùng nó mà không cần biết chi tiết bên trong.

## Decision rules
- Loại bỏ `if-else` lớn dựa trên loại xử lý.
- Giúp thêm thuật toán mới mà ít sửa code cũ.
- Kết hợp rất tốt với `OCP` và `DIP`.
- Khi cùng một tác vụ có nhiều rule hoặc thuật toán thay thế nhau.
- Khi muốn chọn behavior theo config, user type, hoặc runtime state bên ngoài.
- Bỏ qua nếu chỉ có một thuật toán ổn định và không có dấu hiệu biến thiên.
- Tránh tạo quá nhiều strategy cho bài toán rất nhỏ.
- Tăng số lượng class.
- Nếu strategy quá nhỏ và ít thay đổi, abstraction có thể không đáng chi phí.

## Example
```csharp
public interface IDiscountStrategy
{
    decimal Apply(decimal price);
}
public class RegularDiscount : IDiscountStrategy
{
    public decimal Apply(decimal price) => price;
}
public class VipDiscount : IDiscountStrategy
{
    public decimal Apply(decimal price) => price * 0.8m;
}
public class CheckoutService
{
    private readonly IDiscountStrategy _discountStrategy;
    public CheckoutService(IDiscountStrategy discountStrategy)
    {
        _discountStrategy = discountStrategy;
    }
    public decimal Checkout(decimal price)
    {
        return _discountStrategy.Apply(price);
    }
}
```

## Related notes
- [[Definition]]
- [[../SOLID/Open Closed Principle|Open Closed Principle]]
- [[Summary]]
