Default
```kotlin
@Composable
fun ButtonExample(onButtonClicked: () -> Unit) {
  Button(onClick = onButtonClicked) {
    Text(text = "Send")
  }
}
```

Click event toast
```kotlin
class MainActivity : ComponentActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent {
      ComposePracticeTheme {
        ButtonExample {
          Toast.makeText(this, "show toast", Toast.LENGTH_SHORT).show() // 여기에서 버튼 이벤트 정의
        }
      }
    }
  }
}

@Composable
fun ButtonExample(onButtonClicked: () -> Unit) {
  Button(onClick = {}) {
    Text(text = "Send")
  }
}
```

Icon 추가
```kotlin
@Composable
fun ButtonExample(onButtonClicked: () -> Unit) {
  Button(onClick = onButtonClicked) {
    Icon(
      imageVector = Icons.Filled.Send,    // 이미지
      contentDescription = "something send"  // 벡터 이미지 설명
    )
    Text(text = "Send")
  }
}
```

Icon, Text 사이에 Spacer 추가
```kotlin
@Composable
fun ButtonExample(onButtonClicked: () -> Unit) {
  Button(onClick = onButtonClicked) {
    Icon(
      imageVector = Icons.Filled.Send,    // 이미지
      contentDescription = "something send"  // 벡터 이미지 설명
    )
    Spacer(modifier = Modifier.size(ButtonDefaults.IconSpacing))
//    Spacer(modifier = Modifier.size(30.dp))
    Text(text = "Send")
  }
}
```
