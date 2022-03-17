# EditText 없이 강제 SoftKeyboard 쓰기

## Manifest 설정

```xml
        <activity
            android:name=".lobby.LobbyActivity"
            android:exported="false"
            
            android:windowSoftInputMode="stateAlwaysVisible" 

            android:screenOrientation="landscape" />
```
- windowSoftInputMode 설정 추가

<br>

## 키 입력 수신

``` kotlin
    override fun onKeyDown(keyCode: Int, event: KeyEvent?): Boolean {
        report("keyCode : $keyCode :: keyEvent : ${event?.action} :: unicodechar : ${event?.unicodeChar}")

        return super.onKeyDown(keyCode, event)
    }
```
- event?.unicodeChar : 입력받은 key의 unicode

---

# 버튼이 항상 SoftKeyboard 위에 있도록
Button의 옵션에 `android:windowSoftInputMode="adjustResize"` 추가
