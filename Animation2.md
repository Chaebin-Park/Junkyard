# 1초뒤 서서히 사라지는 애니메이션

```kotlin
        val alphaAnimation = AlphaAnimation(1.0f, 0.0f) // alpha값 1.0 ~ 0.0까지
        alphaAnimation.apply {
            startOffset = 1000  // 1초 대기
            duration = 2000     // alpha값
            fillAfter = true    // animation이 끝난 상태 유지
        }
        bind.view.startAnimation(alphaAnimation)
```
