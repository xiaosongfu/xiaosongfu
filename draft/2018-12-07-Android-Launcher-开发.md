
### make our app into a launcher



为主 Activity 设置 


The first item on our agenda is to make our app into a launcher. That means making sure the Android system identifies it as such, loads it up when the system first boots, and shows it whenever you hit the “home” button.

This is simple — you just need to add the following two lines to your Android manifest file inside the activity tag:


```
<category android:name="android.intent.category.HOME" />
<category android:name="android.intent.category.DEFAULT" />
```



---

How to build a custom launcher in Android Studio – Part One
https://www.androidauthority.com/make-a-custom-android-launcher-837342-837342/

How to build a custom launcher in Android Studio – Part Two
https://www.androidauthority.com/custom-launcher-part-two-838188/