---
aliases:
  - Abstract Model
note_type: feature
tags:
  - basic
---

## Core idea
- `Abstraction` là nguyên tắc chỉ giữ lại phần quan trọng của mô hình và ẩn bớt chi tiết cài đặt không cần thiết.

## Key points
- `Abstraction` giúp bạn làm việc với một concept ở mức phù hợp thay vì luôn phải quan tâm chi tiết bên trong.
- Nó xuất hiện qua `interface`, `abstract class`, hoặc API mô tả “làm gì” hơn là “làm như thế nào”.
- Bạn định nghĩa contract chung như `IPaymentProcessor`.
- Code sử dụng chỉ biết gọi `Pay`, còn chi tiết thanh toán bằng thẻ hay ví điện tử được ẩn trong implementation cụ thể.
- Nhờ đó, nơi sử dụng tập trung vào intent thay vì chi tiết kỹ thuật.

## Decision rules
- Giảm coupling giữa nơi gọi và nơi cài đặt.
- Làm code dễ đọc hơn vì bám vào khái niệm của domain.
- Tạo nền cho việc thay implementation hoặc test bằng fake/mock.
- Khi có nhiều implementation khác nhau cho cùng một vai trò.
- Khi bạn muốn tách phần business logic khỏi chi tiết hạ tầng hoặc kỹ thuật.
- Tránh tạo abstraction quá sớm nếu hiện tại chỉ có một implementation rất đơn giản và chưa có áp lực thay đổi.
- Tránh tạo interface chỉ để “cho có”, vì abstraction vô nghĩa sẽ làm code khó lần theo hơn.
- Abstraction kém chất lượng có thể quá chung chung hoặc quá rò rỉ chi tiết implementation.
- Nếu tạo nhiều layer không cần thiết, code sẽ bị vòng vo.

## Example
```csharp
public interface IPaymentProcessor
{
    void Pay(decimal amount);
}
public class CreditCardProcessor : IPaymentProcessor
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} by credit card.");
    }
}
public class CheckoutService
{
    private readonly IPaymentProcessor _paymentProcessor;
    public CheckoutService(IPaymentProcessor paymentProcessor)
    {
        _paymentProcessor = paymentProcessor;
    }
    public void Checkout(decimal amount)
    {
        _paymentProcessor.Pay(amount);
    }
}
```

## Related notes
- [[Definition]]
- [[Polymorphism]]
- [[Summary]]
