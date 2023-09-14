```kotlin
  /**비트맵을 바이너리 바이트배열로 바꾸어주는 메서드  */
  fun bitmapToByteArray(bitmap: Bitmap): String {
      var image = ""
      val stream = ByteArrayOutputStream()
      bitmap.compress(Bitmap.CompressFormat.JPEG, 100, stream)
      val byteArray = stream.toByteArray()
      image = "&image=" + byteArrayToBinaryString(byteArray)
      return image
  }
  
  /**바이너리 바이트 배열을 스트링으로 바꾸어주는 메서드  */
  fun byteArrayToBinaryString(b: ByteArray): String {
      val sb = StringBuilder()
      for (i in b.indices) {
          sb.append(byteToBinaryString(b[i]))
      }
      return sb.toString()
  }
  
  /**바이너리 바이트를 스트링으로 바꾸어주는 메서드  */
  fun byteToBinaryString(n: Byte): String {
      val sb = StringBuilder("00000000")
      for (bit in 0..7) {
          if (n.toInt() shr bit and 1 > 0) {
              sb.setCharAt(7 - bit, '1')
          }
      }
      return sb.toString()
  }
```
