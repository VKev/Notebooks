---
aliases:
  - Observer Pattern
note_type: feature
tags:
  - basic
---

## One-line
- `Observer` cho phép một object phát thông báo cho nhiều object khác khi state hoặc event thay đổi.

## What is it
- `Observer` là behavioral pattern.
- Nó tạo quan hệ publish-subscribe giữa subject và observer.

## How it works
- Subject giữ danh sách listener hoặc event subscribers.
- Khi có thay đổi, subject phát thông báo tới tất cả observer đã đăng ký.
- Observer phản ứng mà không cần subject biết chi tiết logic của từng observer.

## Why use it
- Giảm coupling giữa nơi phát sự kiện và nơi xử lý sự kiện.
- Rất hợp cho UI update, notification, analytics, hoặc event-driven flow.

## When to use it
- Dùng khi một thay đổi cần thông báo cho nhiều phần khác nhau.
- Dùng khi bạn muốn mở rộng reaction mà không sửa object phát sự kiện quá nhiều.

## When to not use it
- Không nên dùng nếu chỉ có một nơi gọi trực tiếp đơn giản là đủ.
- Không nên để event flow quá ẩn nếu team khó theo dõi source của side effect.

## Limitations
- Dễ gây khó debug nếu có quá nhiều subscriber.
- Nếu unsubscribe không đúng, có thể gây leak hoặc giữ reference lâu hơn mong muốn.

---

## Example code
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

---

## Related notes
- [[Definition]]
- [[Command]]
- [[Summary]]
