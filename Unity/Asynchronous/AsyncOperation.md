---
aliases:
  - Unity AsyncOperation
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `AsyncOperation` là kiểu operation bất đồng bộ của Unity, dùng cho scene loading, resource loading, và các tác vụ engine-side cần theo dõi tiến độ hoặc thời điểm hoàn thành.

## Key points
- `AsyncOperation` là class của Unity đại diện cho một operation bất đồng bộ.
- dùng trong coroutine với `yield return`, theo dõi bằng `progress`, kiểm tra bằng `isDone`, hoặc bắt completion qua event `completed`.
- Theo manual Unity `6.4`, mọi type kế thừa `AsyncOperation` cũng nằm trong nhóm API hỗ trợ `await` qua hệ `Awaitable`.
- Unity khởi động operation ở engine side và trả về một `AsyncOperation` để bạn theo dõi.
- Trong coroutine, bạn có thể `yield return` trực tiếp object này cho tới khi xong.
- Trong flow async hiện đại, bạn có thể `await` các operation phù hợp như `SceneManager.LoadSceneAsync(...)`.
- Với scene loading, bạn còn có thể điều khiển `allowSceneActivation` để tách giai đoạn load và activate.

## Decision rules
- Khi gọi các API như `LoadSceneAsync`, `Resources.LoadAsync`, hoặc `AssetBundle.LoadAssetAsync`.
- Khi bạn cần progress bar, loading screen, hoặc kiểm soát thời điểm activate scene.
- Không cần tách riêng thành `AsyncOperation` nếu công việc chỉ là chờ thời gian hoặc chờ frame, vì khi đó `Coroutine` hoặc `Awaitable` frame API hợp hơn.
- Không phù hợp cho logic thuần tính toán của bạn vì abstraction cho operation từ phía Unity engine.
- Nếu không quản lý `allowSceneActivation` cẩn thận, flow load scene có thể gây hiểu nhầm về thời điểm hoàn tất.
- dạng chuẩn của nhiều Unity async API nên quan trọng khi làm việc với scene, asset, và loading flow.
- Dễ theo dõi tiến độ và trạng thái hoàn thành.
- Linh hoạt vì dùng được cả trong coroutine lẫn `await`-based flow.

## Example
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

## Related notes
- [[Definition]]
- [[Coroutine]]
- [[Awaitable]]
- [[Summary]]
