# 코드상에서 animation 추가하기
배너 밑에서 올라오게 하는거
```kotlin
  private fun showAnimTopBanner(message: String, @ColorInt color: Int) {
        val animate = TranslateAnimation(0f, 0f, 0f, -binding.banner.height.toFloat()).apply {
            duration = 2000
            fillAfter = true
            setAnimationListener(object : Animation.AnimationListener {
                override fun onAnimationRepeat(animation: Animation?) = Unit
                override fun onAnimationStart(animation: Animation?) = run { binding.banner.visibility = View.VISIBLE }
                override fun onAnimationEnd(animation: Animation?) = run { binding.banner.visibility = View.INVISIBLE }
            })
        }
        binding.banner.text = message
        binding.banner.setBackgroundColor(color)
        binding.banner.startAnimation(animate)
    }
```

# xml로 만들기
## anim/fade_in_from_bottom.xml
``` xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromYDelta="100%p"
        android:toYDelta="0%p"
        android:duration="500"
        android:repeatCount="0"
        android:fillAfter="true"/>
</set>
```

## anim/fade_in_from_right.xml
``` xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromXDelta="100%p"
        android:toXDelta="0%p"
        android:duration="500"
        android:repeatCount="0"
        android:fillAfter="true"/>
</set>
```

## 적용하기
```java
    private Animation fadeInAnim;
    private Animation fadeOutAnim;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // 이렇게 쓰거나
        fadeInAnim = AnimationUtils.loadAnimation(this, R.anim.fade_in_from_right);
        bind.view.startAnimation(fadeInAnim);
        
        // 액티비티 전환 애니메이션 적용방법
        // overridePendingTransition(a, b)
        // a : 다음 액티비티가 나타나면서 할 애니메이션
        // b : 지금 액티비티가 사라지면서 할 애니메이션
        overridePendingTransition(R.anim.fade_in_from_right, R.anim.fade_out_to_left);
    }
```
자바 코드긴 한데 코들린으로 걍 바꾸면 될듯. sdk version 23에서 한거라 지금이랑 안맞을수도 있음
