---
aliases:
  - ISP
note_type: feature
tags:
  - basic
---

## One-line
- `Interface Segregation Principle` yêu cầu client chỉ nên phụ thuộc vào những method mà nó thực sự cần.

## What is it
- `ISP` khuyên bạn chia interface lớn thành nhiều interface nhỏ, đúng mục đích sử dụng.
- Client không nên bị ép implement hoặc phụ thuộc những method không liên quan đến nó.

## How it works
- Thay vì một interface “đa năng”, bạn tách theo capability.
- Class nào cần in thì implement `IPrinter`, class nào cần scan thì implement `IScanner`.
- Client chỉ nhận đúng abstraction nó cần.

## Why use it
- Giảm coupling không cần thiết.
- Tránh class phải implement method rỗng hoặc method không hợp ngữ cảnh.
- Làm contract dễ hiểu và focused hơn.

## When to use it
- Dùng khi interface bắt đầu phình to và phục vụ nhiều loại client rất khác nhau.
- Dùng khi một số implementation chỉ dùng được một phần nhỏ của interface hiện tại.

## When to not use it
- Không nên tách quá nhỏ đến mức mỗi interface chỉ còn một method vô nghĩa nếu chúng luôn đi cùng nhau.
- Không cần chia interface nếu tất cả client thật sự đều cần toàn bộ contract đó.

## Limitations
- Quá nhiều interface nhỏ có thể làm code khó điều hướng hơn.
- Cần cân bằng giữa interface focused và interface bị chia vụn.

---

## Example code
```csharp
public interface IPrinter
{
    void Print(string document);
}

public interface IScanner
{
    void Scan(string document);
}

public class SimplePrinter : IPrinter
{
    public void Print(string document)
    {
        Console.WriteLine($"Print: {document}");
    }
}

public class OfficeMachine : IPrinter, IScanner
{
    public void Print(string document)
    {
        Console.WriteLine($"Print: {document}");
    }

    public void Scan(string document)
    {
        Console.WriteLine($"Scan: {document}");
    }
}
```

---

## Related notes
- [[Definition]]
- [[Dependency Inversion Principle]]
- [[Summary]]
