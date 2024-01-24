Default
```kotlin
fun Greeting(name: String) {
  Text(text = "Hello $name")
}
```

Color
```kotlin
fun Greeting(name: String) {
  Text(color = Color.Red, text = "Hello $name")
}
```

Color2
```kotlin
fun Greeting(name: String) {
  Text(color = Color(0xffff9944), text = "Hello $name")
}
```

Font size
```kotlin
fun Greeting(name: String) {
  Text(color = Color(0xffff9944), text = "Hello $name", fontSize = 30.sp)
}
```

Font weight
```kotlin
fun Greeting(name: String) {
  Text(color = Color(0xffff9944), text = "Hello $name", fontSize = 30.sp, fontWeight = FontWeight.Bold)
}
```

Font family
```kotlin
fun Greeting(name: String) {
  Text(
    color = Color(0xffff9944),
    text = "Hello $name",
    fontSize = 30.sp,
    fontWeight = FontWeight.Bold,
    fontFamily = FontFamily.Cursive
  )
}
```

Letter spacing
```kotlin
fun Greeting(name: String) {
  Text(
    color = Color(0xffff9944),
    text = "Hello $name",
    fontSize = 30.sp,
    fontWeight = FontWeight.Bold,
    fontFamily = FontFamily.Cursive,
    letterSpacing = 2.sp
  )
}
```

Max lines
```kotlin
fun Greeting(name: String) {
  Text(
    color = Color(0xffff9944),
    text = "Hello $name\nHello $name\nHello $name",
    fontSize = 30.sp,
    fontWeight = FontWeight.Bold,
    fontFamily = FontFamily.Cursive,
    letterSpacing = 2.sp,
    maxLines = 2
  )
}
```

Text decoration
```kotlin
fun Greeting(name: String) {
  Text(
    color = Color(0xffff9944),
    text = "Hello $name\nHello $name\nHello $name",
    fontSize = 30.sp,
    fontWeight = FontWeight.Bold,
    fontFamily = FontFamily.Cursive,
    letterSpacing = 2.sp,
    maxLines = 2,
    textDecoration = TextDecoration.Underline
  )
}
```

Text align, modifier
```kotlin
fun Greeting(name: String) {
  Text(
    modifier = Modifier.width(300.dp),
    color = Color(0xffff9944),
    text = "Hello $name\nHello $name\nHello $name",
    fontSize = 30.sp,
    fontWeight = FontWeight.Bold,
    fontFamily = FontFamily.Cursive,
    letterSpacing = 2.sp,
    maxLines = 2,
    textDecoration = TextDecoration.Underline,
    textAlign = TextAlign.Center
  )
}
```


