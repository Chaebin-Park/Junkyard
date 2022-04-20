# TextView 텍스트 일부만 색, 크기, 스타일 바꾸기
```kotlin
    private fun changeTextViewColor(word: String) {
        val text: String = bind.tvCreateSpaceStep.text.toString()
        val spannableString = SpannableString(text)

        val start: Int = text.indexOf(word)
        val end: Int = start + word.length

        spannableString.setSpan(ForegroundColorSpan(ContextCompat.getColor(this@CreateSpaceActivity, R.color.highlight_blue)), start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE)  // TextColor
        spannableString.setSpan(StyleSpan(Typeface.BOLD), start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE) // Text Style
        spannableString.setSpan(RelativeSizeSpan(1.3f), start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE) // Text Size
        
        bind.tvCreateSpaceStep.text = spannableString
    }
```
