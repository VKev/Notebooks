---
aliases:
  - Language Integrated Query
  - Deferred Execution
note_type: feature
tags:
  - basic
---

## Core idea
- `LINQ` là tập tính năng C# cho phép query, filter, sort, group, và transform data bằng syntax thống nhất.

## Key points
- `LINQ` tích hợp query vào C# thay vì viết query bằng string rời rạc.
- Query có thể viết bằng query syntax hoặc method syntax.
- LINQ làm việc với `IEnumerable<T>` cho in-memory data và `IQueryable<T>` cho provider có thể dịch query sang hệ khác.
- Query expression được compiler chuyển thành standard query operator method call.
- Với `IEnumerable<T>`, query chạy bằng delegate trên local collection.
- Với `IQueryable<T>`, query được biểu diễn bằng expression tree để provider dịch sang query khác như SQL.
- Nhiều LINQ operator dùng deferred execution, nghĩa là query chưa chạy cho tới khi enumerate bằng `foreach`, `ToList()`, `Count()`, hoặc operator terminal khác.

## Decision rules
- Giúp code query data ngắn gọn và type-safe.
- Dễ đọc cho các bước filter, projection, ordering, grouping.
- Giảm boilerplate khi transform collection.
- Dùng cho code xử lý collection không nằm trong hot path.
- Khi mục tiêu chính là readability và transform data rõ ràng.
- Dùng `ToList()` hoặc materialize có chủ đích khi cần snapshot kết quả một lần.
- Tránh dùng LINQ trong hot path như `Update` mỗi frame nếu allocation hoặc deferred execution gây khó kiểm soát.
- Không dùng LINQ khi loop rõ ràng hơn, ít allocate hơn, hoặc dễ debug hơn.
- Không dùng query có side effect ẩn vì deferred execution có thể khiến side effect chạy nhiều lần.
- Deferred execution dễ gây nhầm nếu source collection thay đổi trước lúc enumerate.
- Một query có thể chạy lại nhiều lần nếu enumerate nhiều lần.
- Với `IQueryable`, không phải mọi method C# đều dịch được sang provider backend.

## Example
```csharp
using System.Collections.Generic;
using System.Linq;
public class ScoreBoard
{
    public IReadOnlyList<int> GetTopScores(IEnumerable<int> scores)
    {
        return scores
            .Where(score => score >= 80)
            .OrderByDescending(score => score)
            .Take(3)
            .ToList();
    }
}
```

## Related notes
- [[Definition]]
- [[Generics]]
- [[Delegate and Event]]
- [[Summary]]
