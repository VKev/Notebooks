---
aliases:
  - SRP
note_type: feature
tags:
  - basic
---

## One-line
- `Single Responsibility Principle` yêu cầu một class nên có một trách nhiệm chính, tức một lý do chính để thay đổi.

## What is it
- `SRP` không nói rằng class phải thật nhỏ, mà nói rằng trách nhiệm của class phải tập trung.
- Nếu một class vừa tính toán, vừa lưu dữ liệu, vừa in báo cáo, class đó thường có nhiều lý do để thay đổi.

## How it works
- Bạn tách các concern khác nhau thành các class riêng.
- Mỗi class xử lý một loại thay đổi chính.
- Khi rule tính toán đổi, bạn sửa class tính toán; khi format in đổi, bạn sửa class in.

## Why use it
- Làm class dễ hiểu và dễ test hơn.
- Giảm nguy cơ một thay đổi kéo theo nhiều side effect không liên quan.
- Giúp codebase dễ chia nhỏ trách nhiệm theo module.

## When to use it
- Dùng khi một class bắt đầu phình to và chứa nhiều nhóm method khác nhau.
- Dùng khi bạn thấy cùng một file bị sửa bởi nhiều lý do nghiệp vụ khác nhau.

## When to not use it
- Không cần tách class quá sớm nếu logic vẫn còn rất nhỏ và cùng một trách nhiệm.
- Không nên chia class thành quá nhiều mảnh vụn chỉ để “đúng SRP” trên hình thức.

## Limitations
- Tách quá mức có thể làm flow khó theo dõi hơn.
- `SRP` đòi hỏi hiểu đúng domain; nếu xác định sai responsibility, bạn vẫn tách sai.

---

## Example code
```csharp
public class InvoiceCalculator
{
    public decimal CalculateTotal(decimal price, int quantity)
    {
        return price * quantity;
    }
}

public class InvoicePrinter
{
    public void Print(decimal total)
    {
        Console.WriteLine($"Invoice total: {total}");
    }
}
```

---

## Related notes
- [[Definition]]
- [[Open Closed Principle]]
- [[Summary]]
