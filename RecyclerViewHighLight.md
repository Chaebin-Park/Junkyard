# 화면 중앙에 위치한 RecyclerView 강조하기

## RecylcerView.OnScrollListener 정의
```kotlin
private var prevCenterPos: Int = 2 // 가장 처음 강조할 Item 위치로 설정하면 됨.

private val onScrollListener = object : RecyclerView.OnScrollListener() {
        override fun onScrolled(recyclerView: RecyclerView, dx: Int, dy: Int) {
            super.onScrolled(recyclerView, dx, dy)

            val center: Float = (recyclerView.width / 2).toFloat()
            val centerView = recyclerView.findChildViewUnder(center, recyclerView.top.toFloat())
            val centerPos = centerView?.let { recyclerView.getChildAdapterPosition(it) }

            if (prevCenterPos != centerPos) {
                val prevView = recyclerView.layoutManager?.findViewByPosition(prevCenterPos)
                
                // 이전에 강조된 Item이 있다면 Dehighlight
                if (prevView != null) {
                    val viewHolder = recyclerView.findViewHolderForAdapterPosition(prevCenterPos)

                    // Background 설정. Header, Footer 말고 Item만 효과 적용하려고 조건 추가함.
                    when (viewHolder) {
                        is MyViewAdapter.ItemViewHolder -> {
                            prevView.background = ContextCompat.getDrawable(this@LobbyActivity, R.drawable.basic_item_background)
                        }
                    }

                    // Item 크기 조절. 원래 사이즈로 돌려놓기
                    val itemView = viewHolder?.itemView
                    val layoutParams = itemView?.layoutParams
                    layoutParams?.apply {
                        height = 600
                        width = 600
                    }
                    itemView?.layoutParams = layoutParams
                }

                // 현재 Center에 있는 Item Highlight
                if (centerView != null) {
                    val viewHolder = centerPos?.let { recyclerView.findViewHolderForAdapterPosition(it) }

                    // Background 설정. Header, Footer 말고 Item만 효과 적용하려고 조건 추가함.
                    when (viewHolder) {
                        is MyViewAdapter.ItemViewHolder -> {
                            centerView.background = ContextCompat.getDrawable(this@LobbyActivity, R.drawable.highlighted_item_background)
                        }
                    }
                    
                    // Item 크기 조절. Highlight하려고 크기 키움
                    val itemView = viewHolder?.itemView
                    val layoutParams = itemView?.layoutParams
                    layoutParams?.apply {
                        height = 700
                        width = 700
                    }
                    itemView?.layoutParams = layoutParams
                }
            }

            // Center Position 최신화
            if (centerPos != null) {
                prevCenterPos = centerPos
            }
        }

        // 스크롤 상태 변화 체크할 수 있음. Item을 무조건 중앙에 위치시킬수도 있을듯?
        override fun onScrollStateChanged(recyclerView: RecyclerView, newState: Int) {
            super.onScrollStateChanged(recyclerView, newState)

        }
    }
```

## RecyclerView Initialize
```kotlin
private lateinit var adapter: LobbySpaceRecyclerViewAdapter
private lateinit var decoration: LobbySpaceRecyclerViewDecoration

private fun initRecyclerView(data: List<Space>) {
        val layoutManager = LinearLayoutManager(this@LobbyActivity)
        layoutManager.orientation = LinearLayoutManager.HORIZONTAL

        adapter = MyRecyclerViewAdapter(this@LobbyActivity)
        adapter.dataList.addAll(data) // 데이터 리스트 설정
        adapter.setOnItemClickListener(onItemClickListener) // 클릭 이벤트 리스너 설정

        bind.rvLobbySpaceList.let {
            it.layoutManager = layoutManager
            it.adapter = adapter
            
            // RecyclerView를 새로고침 할때마다 Decoration이 중첩되어서 기존 Decoration이 있으면 지우고 새로 붙여줌
            if (this::decoration.isInitialized)
                it.removeItemDecoration(this.decoration)
            decoration = MyRecyclerViewDecoration(70)                     
            it.addItemDecoration(decoration)
                       
            it.addOnScrollListener(onScrollListener) // 위에서 만들어둔 ScrollListener 추가
            
            // Header 말고 첫번째 데이터부터 보려고 시작지점 설정함.
            // 데이터가 없으면 어쩔수없이 헤더부터 봐야함.
            if (data.isNotEmpty())
                it.scrollToPosition(1)
            else
                it.scrollToPosition(0)
        }
    }
```
