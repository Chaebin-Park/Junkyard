# RecyclerView 스크롤 시 아이템 항상 중앙에 오도록 하는거
```kotlin
    fun RecyclerView.getScrollDistanceOfColumnClosestToLeft(): Int {
        val manager = layoutManager as LinearLayoutManager?
        val firstVisibleColumnViewHolder = findViewHolderForAdapterPosition(
            manager!!.findFirstVisibleItemPosition()
        ) ?: return 0
        val columnWidth = firstVisibleColumnViewHolder.itemView.measuredWidth
        val left = firstVisibleColumnViewHolder.itemView.left
        val absoluteLeft = Math.abs(left)
        return if (absoluteLeft <= columnWidth / 2) left else columnWidth - absoluteLeft
    }

    private fun RecyclerView.setMagneticMove(nStart : Int = 0 ){
        addOnScrollListener(object:RecyclerView.OnScrollListener(){
            var oldMoveTo : Int = 0
            override fun onScrollStateChanged(recyclerView: RecyclerView, newState: Int) {
                val moveTo = getScrollDistanceOfColumnClosestToLeft() - nStart
                if(newState == RecyclerView.SCROLL_STATE_IDLE){
                    if(moveTo != oldMoveTo){
                        recyclerView.smoothScrollBy(moveTo, 0)
                        oldMoveTo = moveTo
                    }
                }
            }
        })
    }
```

# RecyclerView 특정 position으로 이동 시 부드럽게 이동하도록
```kotlin
    @Throws(IllegalArgumentException::class)
    private fun smoothScroll(rv: RecyclerView, toPos: Int, duration: Int) {
        val TARGET_SEEK_SCROLL_DISTANCE_PX =
            10000
        var itemWidth = resources.getDimension(R.dimen.lobby_item_highlight_width).toInt()
        itemWidth += 33
        val fvPos =
            (rv.layoutManager as LinearLayoutManager?)!!.findFirstCompletelyVisibleItemPosition()
        var i = abs((fvPos - toPos) * itemWidth)
        if (i == 0) {
            i = abs(resources.getDimension(R.dimen.lobby_item_highlight_width).toInt())
        }
        val totalPix = i
        val smoothScroller: SmoothScroller = object : LinearSmoothScroller(rv.context) {
            override fun getVerticalSnapPreference(): Int {
                return SNAP_TO_START
            }

            override fun calculateTimeForScrolling(dx: Int): Int {
                var ms = (duration * dx / totalPix.toFloat()).toInt()
                if (dx < TARGET_SEEK_SCROLL_DISTANCE_PX) {
                    ms *= 2
                }
                return ms
            }
        }

        smoothScroller.targetPosition = toPos
        rv.layoutManager!!.startSmoothScroll(smoothScroller)
    }
```
