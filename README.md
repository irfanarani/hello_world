# hello_world
minproject development about biometric
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
package="com.example.biometrics">
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.USE_FINGERPRINT"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-feature
android:name="android.hardware.fingerprint"
android:required="true" />
<application
android:allowBackup="true"
android:icon="@mipmap/ic_launcher"
android:label="@string/app_name"
android:roundIcon="@mipmap/ic_launcher_round"
android:supportsRtl="true"
android:theme="@style/AppTheme">
<activity
android:name=".activities.MainActivity"
android:screenOrientation="portrait">
<intent-filter>
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity>
<activity
android:name=".activities.SignActivity"
android:exported="false"
android:noHistory="true"
android:autoRemoveFromRecents="true"
android:excludeFromRecents="true"
android:screenOrientation="portrait" />
<activity
android:name=".activities.AdminActivity"
android:exported="false"
android:screenOrientation="portrait" />
<activity
android:name=".activities.UserActivity"
android:exported="false"
android:windowSoftInputMode="adjustNothing"
android:screenOrientation="portrait"/>
</application>
</manifest>

Biometric Attendance Application Using Android

Gudlavalleru Engineering College
Page| 20
FingerprintActivity.java

package com.example.biometrics.models;
import android.Manifest;
import android.content.Context;
import android.content.pm.PackageManager;
import android.hardware.fingerprint.FingerprintManager;
import android.os.CancellationSignal;
import android.support.v4.app.ActivityCompat;
import android.widget.Toast;
public class FingerprintHandler extends
FingerprintManager.AuthenticationCallback {
public interface OnAuthenticationFailed {
void onAuthenticationFailed();
}
public interface OnAuthenticationSucceeded {
void
onAuthenticationSucceeded(FingerprintManager.AuthenticationResult result);
}
private Context context;
private CancellationSignal cancellationSignal;
private OnAuthenticationSucceeded succeeded;
private OnAuthenticationFailed failed;
public FingerprintHandler(Context context) {
this.context = context;
}
public void startAuth(AuthManager manager,
OnAuthenticationSucceeded succeeded,
OnAuthenticationFailed failed) {
this.failed = failed;
this.succeeded = succeeded;
this.cancellationSignal = new CancellationSignal();
if (ActivityCompat.checkSelfPermission(context,
Manifest.permission.USE_FINGERPRINT) != PackageManager.PERMISSION_GRANTED)
return;
FingerprintManager fingerprintManager =
manager.getFingerprintManager();
FingerprintManager.CryptoObject wrapped = manager.getWrappedCipher();
fingerprintManager.authenticate(wrapped, this.cancellationSignal, 0,
this, null);
}
public void cancelAuth() {
cancellationSignal.cancel();
}
