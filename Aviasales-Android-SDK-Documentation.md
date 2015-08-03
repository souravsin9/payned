Android Template is a simple module project which uses Aviasales SDK to find and book cheap flight tickets. 

## Installation

### Add gradle dependencies 

To add dependencies in the project use the gradle:

```gradle
repositories {
    maven { url 'http://android.aviasales.ru/repositories/' }
}

dependencies {
    compile 'ru.aviasales.template:aviasalesSdkTemplate:1.0.2'
}
```

### Add the Aviasales API keys to your application

In strings.xml create strings `aviasales_marker` and `aviasales_api_token` with your marker and token params. You can get them at [Travelpayouts.com](https://www.travelpayouts.com/developers/api):

```xml
	<string name="aviasales_marker">74590</string>
	<string name="aviasales_api_token">9f16d617b9df8b2b6b5d0372711e9d6b</string>
```

In `AndroidManifest.xml` add `ru.aviasales.marker` and `ru.aviasales.api_token` as a child of the `<application>` element, by inserting them just before the closing tag `</application>`:

```xml
 <meta-data android:name="ru.aviasales.marker" android:value="@string/aviasales_marker"/>
 <meta-data android:name="ru.aviasales.api_token" android:value="@string/aviasales_api_token"/>
```

### Specify permissions

Specify the permissions `INTERNET` and `ACCESS_NETWORK_STATE` by adding `<uses-permission>` elements as children of the `<manifest>` element. 

```xml
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

## Adding template to project 

### Add `AviasalesFragment` to your activity 

Add to main activity layout `activity_main.xml` the `FrameLayout` for fragments 

```xml
 	<FrameLayout
		android:id="@+id/fragment_place"
		android:layout_width="match_parent"
		android:layout_height="match_parent"/>
```

Add fragment to `MainActivity`

```java	
  public class MainActivity extends ActionBarActivity {
  
  	private AviasalesFragment aviasalesFragment;
    ...
  
  	@Override
  	protected void onCreate(Bundle savedInstanceState) {
  		super.onCreate(savedInstanceState);
  
  		AviasalesSDK.getInstance().init(this); // initialization of AviasalesSDK
  		setContentView(R.layout.activity_main);
     
     		initFragment();
  
      ...
  
  	private void initFragment() {
  		FragmentManager fm = getSupportFragmentManager();
  
  		aviasalesFragment = (AviasalesFragment) fm.findFragmentByTag(AviasalesFragment.TAG); // finding fragment by tag
  
  
  		if (aviasalesFragment == null) { 
  			aviasalesFragment = (AviasalesFragment) AviasalesFragment.newInstance();
  		}
  
  		FragmentTransaction fragmentTransaction = fm.beginTransaction(); // adding fragment to fragment manager
  		fragmentTransaction.replace(R.id.fragment_place, aviasalesFragment, AviasalesFragment.TAG);
  		fragmentTransaction.commit();
  	}
```

### Adding onBackPressed 

For proper back navigation between aviasales child fragments add fragment `onBackPressed` inside of activity `onBackPressed` 

```java
	@Override
	public void onBackPressed() {

    ...
		if (!aviasalesFragment.onBackPressed()) {
			super.onBackPressed();
		}
		...
		
	}
```


## Customization

For proper customization of Aviasales Template use `AviasalesTemplateTheme` or extend it.

```xml    
    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            ...
```

and in `styles.xml`

```xml
	<style name="AppTheme" parent="AviasalesTemplateTheme">
            ...
	</style>
```

To change colors of your app override `colorAsPrimary`, `colorAsPrimaryDark` and `colorAviasalesMain`  in `colors.xml`

```xml
    <color name="colorAsPrimary">#3F51B5</color>
    <color name="colorAsPrimaryDark">#3F51B5</color>
    <color name="colorAviasalesMain">#3F51B5</color>

```

### Aviasales template with toolbar 

If you using toolbar in your project or planning to use it, disable actionbar in your theme 

```xml
        <item name="windowActionBar">false</item>
        <item name="android:windowNoTitle">true</item>
```

and add your toolbar instead of actionbar 	

```java	
		mToolbar = (Toolbar) findViewById(R.id.toolbar_actionbar);
		setSupportActionBar(mToolbar);
```

For more information see the [demo project](https://github.com/KosyanMedia/Aviasales-Android-SDK/tree/master/demo)

### [Aviasales Android API](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Aviasales-Android-SDK-API-documentation)
###[Template project screens](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Template-project-screens)