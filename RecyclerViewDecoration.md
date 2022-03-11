# RecyclerView Item 간격 설정하기
## RecyclerView.ItemDecoration 확장
```kotlin
class LobbySpaceRecyclerViewDecoration(var offset: Int): RecyclerView.ItemDecoration() {

    override fun getItemOffsets(
        outRect: Rect,
        view: View,
        parent: RecyclerView,
        state: RecyclerView.State
    ) {
        super.getItemOffsets(outRect, view, parent, state)

        val position = parent.getChildAdapterPosition(view)
        val count = state.itemCount

        // 맨 앞 Item은 왼쪽 마진, 맨 뒤 Item은 왼쪽 및 오른쪽 마진, 나머지(중간) 아이템은 왼쪽 마진만
        when (position) {
            0 -> {
                outRect.left = offset
            }
            count-1 -> {
                outRect.left = offset
                outRect.right = offset
            }
            else -> {
                outRect.left = offset
            }
        }
    }
}
```
