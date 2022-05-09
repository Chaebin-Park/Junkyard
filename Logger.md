# 라이브러리 추가
> implementation 'com.orhanobut:logger:2.2.0'

# Kotlin
```kotlin
class L {
    companion object {
        private const val PRINT_LOG = BuildConfig.DEBUG // 
        private const val TAG = "TAG"
        private var isInit = false
        private val logFileName: String = App.INSTANCE.logFileName

        fun wtf(log: String?) {
            if (PRINT_LOG) {
                init()
                Logger.wtf(log ?: "null")
            }
        }

        fun e(log: String?) {
            if (PRINT_LOG) {
                init()
                Logger.e(log ?: "null")
            }
        }

        fun d(log: String?) {
            if (PRINT_LOG) {
                init()
                Logger.d(log ?: "null")
            }
        }

        /** * json 문자열을 포맷팅 하여 보여준다 */
        fun js(log: String?) {
            if (PRINT_LOG) {
                init()
                Logger.json(log ?: "null")
            }
        }

        /** * xml 문자열을 포맷팅하여 보여준다 */
        fun xml(log: String?) {
            if (PRINT_LOG) {
                init()
                Logger.xml(log ?: "null")
            }
        }

        val log = StringBuilder()
        fun getLog() {
            if (PRINT_LOG) {
                try {
                    val logcat = Runtime.getRuntime().exec(arrayOf("logcat", "-d"))
                    val br = BufferedReader(InputStreamReader(logcat.inputStream), 4 * 1024)
                    var line = br.readLine()
                    val separator = System.getProperty("line.separator")
                    while (line != null) {
                        if (line.contains(TAG)) {
                            log.append(line)
                            log.append(separator)
                        }
                        line = br.readLine()
                    }
                } catch (e: Exception) {
                }
            }
        }

        fun clearLog() {
            if (PRINT_LOG) {
                try {
                    val process = Runtime.getRuntime().exec("logcat -c")
                } catch (e: IOException) {
                }
            }
        }

        fun saveLogFile(context: Application) {
            if (PRINT_LOG) {
                val path = context.getExternalFilesDir(null)
                val file = File(path, logFileName)
                val stream = FileOutputStream(file)

                try {
                    stream.write(log.toString().toByteArray())
                } catch (e: IOException) {

                } finally {
                    stream.close()
                }
            }
        }

        private fun init() {
            if (!isInit) {
                val formatStrategy = PrettyFormatStrategy.newBuilder()
                    .logStrategy { priority, tag, message ->
                        var last = (10 * Math.random()).toInt()
                        fun randomKey(): String {
                            var random = (10 * Math.random()).toInt()
                            if (random == last) {
                                random = (random + 1) % 10
                            }
                            last = random
                            return random.toString()
                        }
                        Log.println(priority, randomKey() + tag, message)
                    }
                    .tag(TAG)
                    .methodOffset(1)
                    .methodCount(2)
                    .build()
                Logger.addLogAdapter(AndroidLogAdapter(formatStrategy))
                isInit = true
            }
        }
    }
}
```

# 사용법
```kotlin
L.d("로그~~~")
L.e("에러~~~")
```

# 출력
![image](https://user-images.githubusercontent.com/64880435/167515219-75b19aae-7171-4e4b-a89f-40087fdd0d74.png)

- 위   : 쓰레드
- 중간  : 위치
- 아래  : 로그 내용


