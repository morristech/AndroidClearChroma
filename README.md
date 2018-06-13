[![](https://jitpack.io/v/Kunzisoft/AndroidClearChroma.svg)](https://jitpack.io/#Kunzisoft/AndroidClearChroma)

# AndroidClearChroma

<img src="https://raw.githubusercontent.com/Kunzisoft/AndroidClearChroma/master/art/logo.png"> A customisable material color picker view for Android.

<img src="https://raw.githubusercontent.com/Kunzisoft/AndroidClearChroma/master/art/movie1.gif" width="505">
<img src="https://raw.githubusercontent.com/Kunzisoft/AndroidClearChroma/master/art/screen1.png" width="250">

   - supports RGB, ARGB, HSV, HSL, CMYK, CMYK255 color modes (with alpha preview)
   - can indicate current color in either DECIMAL or HEXADECIMAL mode
   - can be used as Dialog, Fragment or as Preference.
   - can select custom shape for preview color in preference
   - add color as part of summary string

## Contribution

You can contribute in different ways to help.

* Add features by a **[pull request](https://help.github.com/articles/about-pull-requests/)**.
* **[Donate](https://www.kunzisoft.com/donation)**  (｡◕‿◕｡)  will be used to create free and open source applications.


## Installation
Add the JitPack repository in your **build.gradle** at the end of repositories:
```
	allprojects {
		repositories {
			...
			maven { url "https://jitpack.io" }
		}
	}
```
And add the dependency
```
	dependencies {
	        compile 'com.github.Kunzisoft:AndroidClearChroma:2.0'
	}
```

### Library version
AndroidClearChroma uses API 25 for the compat library, if you are using a new version for compilation, add this to your **build.gradle** file *(with the right versions)*  : 

```
ext {
    supportlib_version = '26.1.0' // Lib version
}

dependencies {
    ...
    implementation "com.android.support:appcompat-v7:$supportlib_version"
    implementation 'com.github.Kunzisoft:AndroidClearChroma:1.9' // AndroidClearChroma version
     ...
}
```

## Usage

### ChromaDialog

To display a color picker `DialogFragment` from your Activity:
``` java
new ChromaDialog.Builder()
    .initialColor(Color.GREEN)
    .colorMode(ColorMode.ARGB) // RGB, ARGB, HVS, CMYK, CMYK255, HSL
    .indicatorMode(IndicatorMode.HEX) //HEX or DECIMAL; Note that (HSV || HSL || CMYK) && IndicatorMode.HEX is a bad idea
    .create()
    .show(getSupportFragmentManager(), "ChromaDialog");
```

To display a color picker `DialogFragment` from your Fragment:
``` java
new ChromaDialog.Builder()
    .initialColor(Color.GREEN)
    .colorMode(ColorMode.ARGB) // RGB, ARGB, HVS, CMYK, CMYK255, HSL
    .indicatorMode(IndicatorMode.HEX) //HEX or DECIMAL; Note that (HSV || HSL || CMYK) && IndicatorMode.HEX is a bad idea
    .create()
    .show(getChildFragmentManager(), "ChromaDialog");
```

### Listeners

Your parent Activity or Fragment must implement the listener interfaces.

#### OnColorSelectedListener
*OnColorSelectedListener* contains two methods : 
`void onPositiveButtonClick(@ColorInt int color)`called when positiveButton is clicked and
`void onNegativeButtonClick(@ColorInt int color)` called when negativeButton is clicked.

#### OnColorChangedListener
*OnColorChangedListener* contains method
 `void onColorChanged(@ColorInt int color)` called when color is changed in view.
 
 See 
[MainActivity.java](https://github.com/Kunzisoft/AndroidClearChroma/blob/master/sample/src/main/java/com/kunzisoft/androidclearchroma/sample/MainActivity.java)
for complete sample of ChromaDialog

## Style

<img src="https://raw.githubusercontent.com/Kunzisoft/AndroidClearChroma/master/art/screen4.png" width="505">

For custom dialog, simply redefined following nodes : 

```
<style name="Chroma.AlertDialog" parent="Theme.AppCompat.Light.Dialog.Alert">
	<!-- Used for the buttons -->
	<item name="colorAccent">#fff1c8</item>
	<!-- Used for the title and text -->
	<item name="android:textColorPrimary">#c7c7c7</item>
	<!-- Used for the background -->
	<item name="android:background">#353535</item>
 </style>

<style name="Chroma.AlertDialog.Label" parent="TextAppearance.AppCompat.Body1">
	<item name="android:textColor">#a7a7a7</item>
</style>

<style name="Chroma.AlertDialog.Value" parent="TextAppearance.AppCompat.Body2">
	<item name="android:textColor">#939393</item>
</style>

```

### ChromaPreferenceCompat

You must add a `preferenceTheme` node in your activity :
```
<style name="AppTheme.Settings" parent="AppTheme">
	<item name="preferenceTheme">@style/PreferenceThemeOverlay.v14.Material</item>
</style>
```
or (for API < 14)
```
<style name="AppTheme.Settings" parent="AppTheme">
	<item name="preferenceTheme">@style/PreferenceThemeOverlay</item>
</style>
```

<img src="https://raw.githubusercontent.com/Kunzisoft/AndroidClearChroma/master/art/screen3.png" width="250">

**A.** Add Preference to your *.xml preference layout:
``` xml
    <com.kunzisoft.androidclearchroma.ChromaPreferenceCompat
    	xmlns:chroma="http://schemas.android.com/apk/res-auto"
        android:key="hsv" // any key you want
        android:title="HSV sample" // summary will be automatically fetched from the current color
        android:summary="text and [color] string" // add [color] for show current color as string in summary
        chroma:chromaShapePreview="ROUNDED_SQUARE" // CIRCLE, SQUARE, ROUNDED_SQUARE
        chroma:chromaColorMode="HSV" // RGB, ARGB, HSV, HSL, CMYK, CMYK255
        chroma:chromaIndicatorMode="HEX" // HEX or DECIMAL
        chroma:chromaInitialColor="@color/colorAccent"/> // default color
```

**B.** Or you can add preferences dynamically from the code:
```java
    ChromaPreferenceCompat pref = new ChromaPreferenceCompat(getContext());
    pref.setTitle("RGB(added from java)");
    pref.setSummary("Summary ...");
    pref.setColorMode(ColorMode.RGB);
    pref.setIndicatorMode(IndicatorMode.HEX);
    pref.setKey("any_key_you_need");
    getPreferenceScreen().addPreference(pref);
```

You can use `ChromaPreferenceFragmentCompat` for an easier managing of fragment in Preferences.

```
public class ColorPreferenceFragmentCompat extends ChromaPreferenceFragmentCompat {

        @Override
        public void onCreatePreferences(Bundle bundle, String s) {
            addPreferencesFromResource(R.xml.prefs_v7); // load your ChromaPreferenceCompat prefs from xml
        }
    }
```

### Fragment

<img src="https://raw.githubusercontent.com/Kunzisoft/AndroidClearChroma/master/art/screen2.png" width="250">

Simply call it in XML layout with :
```
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="com.kunzisoft.androidclearchroma.fragment.ChromaColorFragment"
        android:id="@+id/activity_color_fragment"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:layout="@layout/chroma_color_fragment" />

</FrameLayout>
```
Unfortunately, you can't customize a fragment in XML. You must initialize the fragment programmatically and use the FragmentManager to add it to your layouts.

Example :
```java
    ChromaColorFragment chromaColorFragment = ChromaColorFragment.newInstance(Color.BLUE, ColorMode.ARGB, IndicatorMode.HEX);
    getSupportFragmentManager()
    .beginTransaction()
    .replace(R.id.container_color_fragment, chromaColorFragment, TAG_COLOR_FRAGMENT)
    .commit();
```
See 
[FragmentColorActivity.java](https://github.com/Kunzisoft/AndroidClearChroma/blob/master/sample/src/main/java/com/kunzisoft/androidclearchroma/sample/FragmentColorActivity.java)
for complete sample.

## Bonus
Method for formatted output of a given color:
```java
ChromaUtil.getFormattedColorString(int color, boolean showAlpha);
```

[Video](https://www.youtube.com/watch?v=zskKV6ifRfw) for create the sample icon (in French)

This project is a fork of [VintageChroma by Pavel Sikun](https://github.com/MrBIMC/VintageChroma).

## License
Copyright 2018 JAMET Jeremy.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
