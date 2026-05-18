---
aliases:
  - Adapter Pattern
note_type: feature
tags:
  - basic
---

## Core idea
- `Adapter` chuyển interface của một class sẵn có thành interface mà hệ thống hiện tại mong đợi.

## Key points
- `Adapter` là structural pattern.
- Nó rất hữu ích khi bạn cần dùng code bên thứ ba hoặc legacy code có API không khớp với hệ thống hiện tại.
- Hệ thống của bạn định nghĩa contract mong muốn như `IPaymentGateway`.
- Adapter implement contract đó và gọi sang API thật của class cũ hoặc thư viện ngoài.
- Nơi sử dụng chỉ biết contract chuẩn, không cần biết chi tiết API gốc.

## Decision rules
- Giúp tích hợp thư viện ngoài mà không lan API lạ ra toàn codebase.
- Giảm chi phí đổi vendor hoặc đổi implementation sau này.
- Khi tích hợp hệ thống có interface không tương thích.
- Khi cần bảo vệ domain code khỏi chi tiết của external API.
- Bỏ qua nếu bạn kiểm soát hoàn toàn cả hai phía và có thể sửa contract trực tiếp.
- Tránh tạo adapter chỉ để đổi tên method đơn giản nếu không có boundary rõ ràng.
- Thêm một lớp trung gian cần bảo trì.
- Nếu API nguồn thay đổi nhiều, adapter cũng phải đổi theo.

## Example
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

## Related notes
- [[Definition]]
- [[Factory Method]]
- [[Summary]]
