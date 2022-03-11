# RecyclerView에 Header, Footer 달기

## Adapter
Adapter의 Generic을 통상 RecyclerView.ViewHolder로 설정
``` kotlin
class MyAdapter(private val context: Context) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {
...
}
```

## ViewHolder
### HeaderViewHolder
```kotlin
class HeaderViewHolder(itemView: View, listener: OnItemClickEventListener) : RecyclerView.ViewHolder(itemView) {
        companion object {
            const val VIEW_TYPE = R.layout.header
        }

        // view 설정
        var textView: TextView = itemView.findViewById(R.id.textview)

        init {
            itemView.setOnClickListener {
                val position = bindingAdapterPosition
                if (position != RecyclerView.NO_POSITION) {
                    listener.onItemClick(itemView, position)
                }
            }
        }
    }
```

### FooterViewHolder
```kotlin
class FooterViewHolder(itemView: View, listener: OnItemClickEventListener) : RecyclerView.ViewHolder(itemView) {
        companion object {
            const val VIEW_TYPE = R.layout.footer
        }

        // view 설정
        var textView: TextView = itemView.findViewById(R.id.textview)

        init {
            itemView.setOnClickListener {
                val position = bindingAdapterPosition
                if (position != RecyclerView.NO_POSITION) {
                    listener.onItemClick(itemView, position)
                }
            }
        }
    }
```

### ItemViewHolder
```kotlin
class ItemViewHolder(itemView: View, listener: OnItemClickEventListener) : RecyclerView.ViewHolder(itemView) {
        companion object {
            const val VIEW_TYPE = R.layout.item
        }

        // view 설정
        var textView: TextView = itemView.findViewById(R.id.textview)

        init {
            itemView.setOnClickListener {
                val position = bindingAdapterPosition
                if (position != RecyclerView.NO_POSITION) {
                    listener.onItemClick(itemView, position)
                }
            }
        }
    }
```

### OnItemClickEventListener
```kotlin
    interface OnItemClickEventListener {
        fun onItemClick(view: View, position: Int)
    }
```
- RecyclerView의 각 Item에 대한 클릭 이벤트

## Adapter Override Method
### getItemViewType
```kotlin
override fun getItemViewType(position: Int): Int {
        return when (position) {
            0 -> HeaderViewHolder.VIEW_TYPE
            dataList.size + 2 -> FooterViewHolder.VIEW_TYPE
            else -> ItemViewHolder.VIEW_TYPE
        }
    }
```

### onCreateViewHolder
```kotlin
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(viewType, parent, false)

        val viewHolder = when (viewType) {
            HeaderViewHolder.VIEW_TYPE -> HeaderViewHolder(view, itemClickListener)
            FooterViewHolder.VIEW_TYPE -> FooterViewHolder(view, itemClickListener)
            else -> ItemViewHolder(view, itemClickListener)
        }

        return viewHolder
    }
```

### onBindViewHolder
```kotlin
override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        when (holder) {
            is HeaderViewHolder -> {
                // Header 설정
            }
            is FooterViewHolder -> {
                // Footer 설정
            }
            else -> {
                // Item 설정
            }
        }
    }
```

### getItemCount
```kotlin
// Item 이외에 추가된 View 갯수만큼 더해주기
override fun getItemCount(): Int = spaceDataList.size + 2
```
