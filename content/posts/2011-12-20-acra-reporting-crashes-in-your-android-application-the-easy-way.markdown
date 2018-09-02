---
author: admin
date: 2011-12-20 20:02:30+00:00
draft: false
title: ACRA, reporting crashes in your Android app the easy way
type: post
url: /blog/2011/12/20/acra-reporting-crashes-in-your-android-application-the-easy-way/
categories:
- Software
- Tecnología
tags:
- acra
- android
- java
- mobile
---

[ACRA](http://code.google.com/p/acra/) (Application Crash Report for Android) is a extremely helpful Android library that allows your mobile application to send a report to different destinations when it crashes -miserably-.

It is both powerful/configurable and very easy to use at the same time, allowing:



	  * Different kind of notifications (silent, toast, status bar, etc)
	  * Detailed crash reports (stack traces, device model, system versions)
	  * Many targets (email, shared google spreadsheet, etc)


In my example I have used a shared google document as the target where the notifications are sent, and I have chosen not to show anything to de user when the application crashes (silent mode).

Once you download and add the acra-x.x.x.jar file to your project, you simply need to annotate your [Application](http://developer.android.com/reference/android/app/Application.html) class:


    
    
    import org.acra.ACRA;
    import org.acra.ReportingInteractionMode;
    import org.acra.annotation.ReportsCrashes;
    import android.app.Application;
    
    @ReportsCrashes(
    		formKey = "<my_google_doc_key>",
    		mode=ReportingInteractionMode.SILENT)
    public class MyApplication extends Application {
    	@Override
    	public void onCreate() {
    		super.onCreate();		
    		ACRA.init(this);
    	}
    }
    



It is important to add the permissions required to use the Internet connection in your AndroidManifest:


    
    
    <uses-permission android:name="android.permission.INTERNET" />
    





### A crash report



Once your application crashes (it will crash), you receive a lot of useful information such as the device status:

`
locale=es_ES
hardKeyboardHidden=HARDKEYBOARDHIDDEN_YES
keyboard=KEYBOARD_NOKEYS
keyboardHidden=KEYBOARDHIDDEN_NO
fontScale=1.0
mcc=214
mnc=7
navigation=NAVIGATION_TRACKBALL
navigationHidden=NAVIGATIONHIDDEN_NO
orientation=ORIENTATION_PORTRAIT
screenLayout=SCREENLAYOUT_SIZE_NORMAL+SCREENLAYOUT_LONG_YES
seq=5
touchscreen=TOUCHSCREEN_FINGER
uiMode=UI_MODE_TYPE_NORMAL+UI_MODE_NIGHT_NO
userSetLocale=false

width=480
height=800
pixelFormat=1
refreshRate=60.0fps
metrics.density=x1.5
metrics.scaledDensity=x1.5
metrics.widthPixels=480
metrics.heightPixels=800
metrics.xdpi=254.0
metrics.ydpi=254.0
`

And the stacktrace, in a transparent way for the user:

`
java.lang.NullPointerException
	at java.util.Collections.sort(Collections.java:1971)
	at net.guidogarcia.SelectActivity$a$1.run(SourceFile:197)
	at android.os.Handler.handleCallback(Handler.java:587)
	at android.os.Handler.dispatchMessage(Handler.java:92)
	at android.os.Looper.loop(Looper.java:144)
	at android.app.ActivityThread.main(ActivityThread.java:4937)
	at java.lang.reflect.Method.invokeNative(Native Method)
	at java.lang.reflect.Method.invoke(Method.java:521)
	at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(...)
	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:626)
	at dalvik.system.NativeStart.main(Native Method)
`

In my case the output seems pretty cryptic because I was also trying to integrate [Proguard](http://proguard.sourceforge.net/) in the application.



### Open points


I am far from being an Android expert, so I would like to know if there is another way to submit this kind of reports that does not need an external library (please use the comments if you know how to do it).

The version I have used is ACRA 3.1.1, so this article might be outdated with the release of ACRA 4.x.
