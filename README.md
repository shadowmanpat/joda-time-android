joda-time-android
=================

This library is a version of [Joda-Time](https://github.com/JodaOrg/joda-time) built with Android in mind.

Why Joda-Time?
==============

Android has built-in date and time handling - why bother with a library?  If you've worked with Java's Date and Calendar classes you can probably answer this question yourself, but if not, check out [Joda-Time's list of benefits](http://www.joda.org/joda-time/#Why_Joda-Time).

For Android developers Joda-Time solves one critical problem: stale timezone data.  Built-in timezone data is only updated when the OS is updated, and we all know how often that happens.  [Countries modify](http://www.bbc.co.uk/news/world-europe-15512177) [their timezones](http://www.heraldsun.com.au/news/breaking-news/samoa-to-move-the-international-dateline/story-e6frf7jx-1226051660380) [all the](http://www.indystar.com/apps/pbcs.dll/article?AID=/20070207/LOCAL190108/702070524/0/LOCAL) [time](http://uk.reuters.com/article/oilRpt/idUKBLA65048420070916); being able to update your own tz data keeps your app up-to-date and accurate.

Why This Library?
=================

I know what you are thinking: Joda-Time is a great library and it's just a single JAR, so why make things more complex by wrapping it in an Android library?

There is a particular problem with the JAR setup on Android: due to its usage of [ClassLoader.getResourceAsStream()](http://developer.android.com/reference/java/lang/ClassLoader.html#getResourceAsStream%28java.lang.String%29), it greatly inflates its memory footprint on apps.  (For more details, see [this blog post](http://blog.danlew.net/2013/08/20/joda_time_s_memory_issue_in_android/).)  This library avoids the problem for Android by loading from resources instead of a JAR.

This library also has extra utilities designed for Android.  For example, see [DateUtils](library/src/main/java/net/danlew/android/joda/DateUtils.java), a port of Android's [DateUtils](http://developer.android.com/reference/android/text/format/DateUtils.html).

Usage
=====

Add the following dependency to `build.gradle`:

```groovy
dependencies {
    compile 'net.danlew:android.joda:2.9.9.1'
}
```

Once that's done, you **must** initialize the library before using it by calling `JodaTimeAndroid.init()`. I suggest putting this code in `Application.onCreate()`:

```java
public class MyApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        JodaTimeAndroid.init(this);
    }
}
```

Troubleshooting
===============
__Q: My build fails with an error about a duplicate file__

> Duplicate files copied in APK META-INF/LICENSE.txt

or

> Duplicate files copied in APK META-INF/NOTICE.txt

__A: We can safely exclude thoses files from our build. You need to specify these two `exclude`s in your `build.gradle` file and you will be good to go:__

```
android {
    ...
    packagingOptions {
        exclude 'META-INF/LICENSE.txt'
        exclude 'META-INF/NOTICE.txt'
    }
}
```
