---
aliases:
  - Adapter Pattern
note_type: feature
tags:
  - basic
---

## One-line
- `Adapter` chuyển interface của một class sẵn có thành interface mà hệ thống hiện tại mong đợi.

## What is it
- `Adapter` là structural pattern.
- Nó rất hữu ích khi bạn cần dùng code bên thứ ba hoặc legacy code có API không khớp với hệ thống hiện tại.

## How it works
- Hệ thống của bạn định nghĩa contract mong muốn như `IPaymentGateway`.
- Adapter implement contract đó và gọi sang API thật của class cũ hoặc thư viện ngoài.
- Nơi sử dụng chỉ biết contract chuẩn, không cần biết chi tiết API gốc.

## Why use it
- Giúp tích hợp thư viện ngoài mà không lan API lạ ra toàn codebase.
- Giảm chi phí đổi vendor hoặc đổi implementation sau này.

## When to use it
- Dùng khi tích hợp hệ thống có interface không tương thích.
- Dùng khi cần bảo vệ domain code khỏi chi tiết của external API.

## When to not use it
- Không cần nếu bạn kiểm soát hoàn toàn cả hai phía và có thể sửa contract trực tiếp.
- Không nên tạo adapter chỉ để đổi tên method đơn giản nếu không có boundary rõ ràng.

## Limitations
- Thêm một lớp trung gian cần bảo trì.
- Nếu API nguồn thay đổi nhiều, adapter cũng phải đổi theo.

---

## Example code
```csharp
public interface IPaymentGateway
{
    void Pay(decimal amount);
}

public class LegacyPaymentService
{
    public void MakePayment(decimal total)
    {
        Console.WriteLine($"Legacy payment: {total}");
    }
}

public class LegacyPaymentAdapter : IPaymentGateway
{
    private readonly LegacyPaymentService _legacyPaymentService;

    public LegacyPaymentAdapter(LegacyPaymentService legacyPaymentService)
    {
        _legacyPaymentService = legacyPaymentService;
    }

    public void Pay(decimal amount)
    {
        _legacyPaymentService.MakePayment(amount);
    }
}
```

---

## Related notes
- [[Definition]]
- [[Factory Method]]
- [[Summary]]
