## Installation

### Add gradle dependencies 

To add Aviasales SDK Library to your project use gradle:

```gradle
repositories {
    maven { url 'http://android.aviasales.ru/repositories/' }
}

dependencies {
    compile 'ru.aviasales.template:aviasalesSdkTemplate:2.0.8-sdk'
}
```

### Initialization of Aviasales SDK

Before any interaction with Aviasales SDK or Aviasales Template you should initialize it 
```java
  		AviasalesSDK.getInstance().init(this, new IdentificationData(TRAVEL_PAYOUTS_MARKER, TRAVEL_PAYOUTS_TOKEN)); 
```

Change `TRAVEL_PAYOUTS_MARKER` and `TRAVEL_PAYOUTS_TOKEN` to your marker and token params. You can get them at [Travelpayouts.com](https://www.travelpayouts.com/developers/api).

### Specify permissions

Don't forget to specify permissions `INTERNET` and `ACCESS_NETWORK_STATE` by adding `<uses-permission>` elements as children of the `<manifest>` element. 

```xml
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

## Example of adding Aviasales Template to your project 

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
  public class MainActivity extends AppCompatActivity {
    //Замените эти константы на ваши маркер и токен из TravelPayouts
    private final static String TRAVEL_PAYOUTS_MARKER = "your_travel_payouts_marker";
    private final static String TRAVEL_PAYOUTS_TOKEN = "your_travel_payouts_token";
    private AviasalesFragment aviasalesFragment;
    ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Инициализация AviasalesSDK. 
        AviasalesSDK.getInstance().init(this, new IdentificationData(TRAVEL_PAYOUTS_MARKER, TRAVEL_PAYOUTS_TOKEN));
        setContentView(R.layout.activity_main);

        initFragment();
    }
      ...

    private void initFragment() {
        FragmentManager fm = getSupportFragmentManager();

        aviasalesFragment = (AviasalesFragment) fm.findFragmentByTag(AviasalesFragment.TAG); // поиск фрагмента по тегу


        if (aviasalesFragment == null) { 
            aviasalesFragment = (AviasalesFragment) AviasalesFragment.newInstance();
        }

        FragmentTransaction fragmentTransaction = fm.beginTransaction(); // добавление фрагмента во FragmentManager
        fragmentTransaction.replace(R.id.fragment_place, aviasalesFragment, AviasalesFragment.TAG);
        fragmentTransaction.commit();
    }
}
```

### Adding `onBackPressed`

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

### Customization

For proper customization of Aviasales Template use `AviasalesTemplateTheme` or extend your theme from it. For example, in `AndroidManifest.xml` specify your theme

```xml    
    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            ...
```

and in `styles.xml` extend your theme from `AviasalesTemplateTheme`

```xml
	<style name="AppTheme" parent="AviasalesTemplateTheme">
            ...
	</style>
```

To change colors of your app override `colorAsPrimary`, `colorAsPrimaryDark` and `colorAviasalesMain`  in `colors.xml`. This is main Aviasales Template colors:

```xml
    <color name="colorAsPrimary">#3F51B5</color>
    <color name="colorAsPrimaryDark">#3F51B5</color>
    <color name="colorAviasalesMain">#3F51B5</color>
```

Also you can change background and price tag colors

```xml
    <color name="aviasales_results_background">@color/grey_background</color>
    <color name="aviasales_search_form_background">@color/white</color>
    <color name="aviasales_filters_background">@color/white</color>

    <color name="aviasales_select_airport_background">@color/white</color>
    <color name="aviasales_ticket_background">@color/grey_background</color>

    <color name="aviasales_results_card_color">@color/white</color>
    <color name="aviasales_price_color">@color/yellow_FDCC50</color>
```

For more information see the [demo project](https://github.com/KosyanMedia/Aviasales-Android-SDK/tree/master/simple_demo)

### [Aviasales Android API](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Aviasales-Android-SDK-API-documentation)
### [Template project screens](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Template-project-screens)