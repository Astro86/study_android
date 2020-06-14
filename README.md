# 안드로이드 공부

## 안드로이드 생명 주기
![](images/android-lifecycle.jpg)

- onCreate
- onStart
- onResume
- onPuase
- onStop
- onRestart
- onDestroy

### onCreate

> Activiy가 처음 실행되는 상태에 제일 먼저 호출되는 메소드이며, 최초로 한번만 실행되는 함수이다.

Activiy가 처음 실행되는 상태에 제일 먼저 호출되는 메소드이며, 최초로 한번만 실행되는 함수이다. 사용자에게 UI를 띄어주기 위한 메소드인 `setContentView`()메소드가 포함되어 있다.

### onStart

> Activity가 사용자에게 보여지기 직전에 호출되는 함수


### onResume

> Activity가 사용자에게 보여지고, 사용자와 상호작용하기 직전에 호출되는 함수

Activiy가 멈춰있다가 다시 호출될 때 불리는 함수, `Pause`상태였다가 다시 호출될 때 불린다.

### onPause
> 현재 사용중인 Activity위에 다른 Activity가 올라와 focus를 잃었을 때 호출되는 함수

### onStop
> 현재 사용중인 Activity가 더 이상 사용자에게 보여지지 않을 경우 호출되는 함수이다.


### onRestart
> Stop된 Activity가 다시 시작할려고 할 때 호출되는 함수이다.

### onDestroy
> Activity가 종료될 때 호출되는 함수이다.