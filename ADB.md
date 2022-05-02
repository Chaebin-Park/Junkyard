# 화면 관련 명령어
- density
> *adb shell wm density*
> 
> Physical density랑 Override density가 나올때가 있는데, Override density에 맞추면 되는듯
- size
> *adb shell wm size*
  - 간혹 독보적인 화면비를 가진 기기들이 있어서 체크해보면 좋다...

## density
|dpi|dp2px|density|비고|
|---|---|---|---|
|ldpi(120dpi)|1dp=0.75px|0.75density||
|mdpi(160dpi)|1dp=1.00px|1.00density|BaseLine|
|hdpi(240dpi)|1dp=1.50px|1.50density||
|xhdpi(320dpi)|1dp=2.00px|2.00density||
|xxdpi(480dpi)|1dp=3.00px|3.00density||
|xxxdpi(640dpi)|1dp=4.00px|4.00density||


## 계산식
dp = px * 160/dpi
px = dp * dpi/160
> ex) density가 212인 경우
> 
> dp = px * 0.75
> 
> px = dp * 1.33...
