# GuestureDetector
> https://developer.android.com/reference/android/view/GestureDetector

스와이프(플링), 더블탭 등 기능을 인식하는 터치패드?가 필요해서 찾은 클래스
- 손가락으로 슥슥 하는거 스와이프인줄 알았는데 플링이라고 하더라...

## GestureDetector.OnGestureListener 인터페이스 구현
``` kotlin

private lateinit var gestureDetector: GestureDetector
private val createGestureDetectorListener = object: GestureDetector.OnGestureListener {
        override fun onDown(p0: MotionEvent?): Boolean {
            report("onDown Call")
            return true
        }

        override fun onShowPress(p0: MotionEvent?) {
            report("onShowPress Call")
        }

        override fun onSingleTapUp(p0: MotionEvent?): Boolean {
            report("onSingleTapUp Call")
            return true
        }

        override fun onScroll(p0: MotionEvent?, p1: MotionEvent?, p2: Float, p3: Float): Boolean {
            report("onScroll Call :: ($p2 * $p3)")
            return true
        }

        override fun onLongPress(p0: MotionEvent?) {
            report("onLongPress Call")
        }

        override fun onFling(p0: MotionEvent?, p1: MotionEvent?, p2: Float, p3: Float): Boolean {
            report("onFling Call :: ($p2 * $p3)")
            return true
        }
}

override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(bind.root)

        gestureDetector = GestureDetector(this, createGestureDetectorListener)

        // GestureView가 따로 있는게 아니라 그냥 View 하나 만들고 TouchListener 달아주면 됨.
        bind.gestureView.setOnTouchListener { view, motionEvent ->
            gestureDetector.onTouchEvent(motionEvent)
            view.performClick()
            true
        }
}
```

## SimpleOnGestureListener 클래스 사용
위에 인터페이스 쓰면 쓰잘데없는 메소드들도 다 구현해야 하니까 안드로이드에서 친절하게 만들어둔 클래스
필요한 메소드만 뽑아서 구현해 쓰면 된다.

```kotlin
private val simpleOnGestureListener = object : SimpleOnGestureListener() {
        override fun onFling(
            e1: MotionEvent?,
            e2: MotionEvent?,
            velocityX: Float,
            velocityY: Float
        ): Boolean {
            return super.onFling(e1, e2, velocityX, velocityY)
        }

        override fun onSingleTapUp(e: MotionEvent?): Boolean {
            return super.onSingleTapUp(e)
        }
}
```
> fling이랑 singletap만 구현한거. 위에 인터페이스랑 똑같이 GestureDetector 파라미터에 넣어주면 된다.


