---
aliases:
  - Data Hiding
note_type: feature
tags:
  - basic
---

## Core idea
- `Encapsulation` là nguyên tắc đóng gói dữ liệu và kiểm soát cách dữ liệu đó bị thay đổi thông qua API rõ ràng.

## Key points
- `Encapsulation` giúp object tự bảo vệ trạng thái nội bộ của nó.
- Thay vì cho code bên ngoài sửa trực tiếp mọi field, object chỉ cho phép tương tác qua method hoặc property phù hợp.
- Dữ liệu quan trọng được để ở `private` hoặc mức truy cập hẹp hơn.
- Object cung cấp method như `Deposit`, `Withdraw`, `SetHealth` để kiểm soát rule khi thay đổi state.
- Nhờ vậy, logic validate và state luôn đi cùng nhau thay vì bị rải ra bên ngoài.

## Decision rules
- Khi class có state cần được bảo vệ hoặc validate.
- Khi bạn muốn gom business rule liên quan đến dữ liệu vào đúng object sở hữu dữ liệu đó.
- Không cần bọc quá mức cho những data carrier đơn giản nếu việc đó không tạo thêm giá trị.
- Tránh tạo getter/setter trống cho mọi field rồi gọi đó là encapsulation, vì như vậy logic vẫn không được bảo vệ.
- Nếu lạm dụng wrapper method vô nghĩa, code sẽ dài mà không rõ hơn.
- Giữ object ở trạng thái hợp lệ dễ hơn.
- Giảm nguy cơ code bên ngoài sửa dữ liệu sai cách.
- Làm API của class rõ ràng hơn: người dùng biết nên làm gì, không nên chạm vào đâu.

## Example
```csharp
public class BankAccount
{
    private decimal _balance;
    public decimal Balance => _balance;
    public void Deposit(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("Amount must be positive.");
        }
        _balance += amount;
    }
    public bool Withdraw(decimal amount)
    {
        if (amount <= 0 || amount > _balance)
        {
            return false;
        }
        _balance -= amount;
        return true;
    }
}
```

## Related notes
- [[Definition]]
- [[Abstraction]]
- [[Summary]]
