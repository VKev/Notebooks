---
aliases:
  - Unity AsyncOperation
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `AsyncOperation` là kiểu operation bất đồng bộ của Unity, thường dùng cho scene loading, resource loading, và các tác vụ engine-side cần theo dõi tiến độ hoặc thời điểm hoàn thành.

## What is it
- `AsyncOperation` là class của Unity đại diện cho một operation bất đồng bộ.
- Nó có thể được dùng trong coroutine với `yield return`, theo dõi bằng `progress`, kiểm tra bằng `isDone`, hoặc bắt completion qua event `completed`.
- Theo manual Unity `6.3`, mọi type kế thừa `AsyncOperation` cũng nằm trong nhóm API hỗ trợ `await` qua hệ `Awaitable`.

## How it works
- Unity khởi động operation ở engine side và trả về một `AsyncOperation` để bạn theo dõi.
- Trong coroutine, bạn có thể `yield return` trực tiếp object này cho tới khi xong.
- Trong flow async hiện đại, bạn có thể `await` các operation phù hợp như `SceneManager.LoadSceneAsync(...)`.
- Với scene loading, bạn còn có thể điều khiển `allowSceneActivation` để tách giai đoạn load và activate.

## Why use it
- Đây là dạng chuẩn của nhiều Unity async API nên rất quan trọng khi làm việc với scene, asset, và loading flow.
- Dễ theo dõi tiến độ và trạng thái hoàn thành.
- Linh hoạt vì dùng được cả trong coroutine lẫn `await`-based flow.

## When to use it
- Dùng khi gọi các API như `LoadSceneAsync`, `Resources.LoadAsync`, hoặc `AssetBundle.LoadAssetAsync`.
- Dùng khi bạn cần progress bar, loading screen, hoặc kiểm soát thời điểm activate scene.

## When to not use it
- Không cần tách riêng thành `AsyncOperation` nếu công việc chỉ là chờ thời gian hoặc chờ frame, vì khi đó `Coroutine` hoặc `Awaitable` frame API hợp hơn.
- Không phù hợp cho logic thuần tính toán của bạn vì đây là abstraction cho operation từ phía Unity engine.

## Limitations
- `AsyncOperation` không phải generic result type cho mọi loại async work của người dùng.
- Nhiều khi nó chỉ đại diện cho trạng thái operation, còn dữ liệu hoặc effect thật sự đến từ API gọi ra nó.
- Nếu không quản lý `allowSceneActivation` cẩn thận, flow load scene có thể gây hiểu nhầm về thời điểm hoàn tất.

---

## Example code
```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class LoadSceneExample : MonoBehaviour
{
    private async Awaitable Start()
    {
        await SceneManager.LoadSceneAsync("GameScene");
    }
}
```

---

## Related notes
- [[Definition]]
- [[Coroutine]]
- [[Awaitable]]
- [[Summary]]
