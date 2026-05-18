---
aliases:
  - Observer Pattern
note_type: feature
tags:
  - basic
---

## Core idea
- `Observer` cho phép một object phát thông báo cho nhiều object khác khi state hoặc event thay đổi.

## Key points
- `Observer` là behavioral pattern.
- Nó tạo quan hệ publish-subscribe giữa subject và observer.
- Subject giữ danh sách listener hoặc event subscribers.
- Khi có thay đổi, subject phát thông báo tới tất cả observer đã đăng ký.
- Observer phản ứng mà không cần subject biết chi tiết logic của từng observer.

## Decision rules
- Giảm coupling giữa nơi phát sự kiện và nơi xử lý sự kiện.
- Rất hợp cho UI update, notification, analytics, hoặc event-driven flow.
- Khi một thay đổi cần thông báo cho nhiều phần khác nhau.
- Khi bạn muốn mở rộng reaction mà không sửa object phát sự kiện quá nhiều.
- Tránh dùng nếu chỉ có một nơi gọi trực tiếp đơn giản là đủ.
- Tránh để event flow quá ẩn nếu team khó theo dõi source của side effect.
- Dễ gây khó debug nếu có quá nhiều subscriber.
- Nếu unsubscribe không đúng, có thể gây leak hoặc giữ reference lâu hơn mong muốn.

## Example
```csharp
public class Order
{
    public string Id { get; set; } = string.Empty;
}
public class OrderService
{
    public event Action<Order>? OrderCreated;
    public void Create(Order order)
    {
        Console.WriteLine($"Create order: {order.Id}");
        OrderCreated?.Invoke(order);
    }
}
public class EmailNotifier
{
    public void Subscribe(OrderService orderService)
    {
        orderService.OrderCreated += HandleOrderCreated;
    }
    private void HandleOrderCreated(Order order)
    {
        Console.WriteLine($"Send email for order: {order.Id}");
    }
}
```

## Related notes
- [[Definition]]
- [[Command]]
- [[Summary]]
