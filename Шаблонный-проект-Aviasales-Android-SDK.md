Шаблонный проект - это простой модульный проект, который использует Aviasales SDK для поиска и бронирования дешевых авиабилетов.

## Установка Шаблонного проекта Aviasales Template

### Добавление зависимостей

Чтобы добавить модуль в проект, используйте gradle:

```gradle
repositories {
    maven { url 'http://android.aviasales.ru/repositories/' }
}

dependencies {
    compile 'ru.aviasales.template:aviasalesSdkTemplate:2.1.8'
}
```

### Инициализация

Перед тем как использовать SDK API или шаблонный проект Aviasales Template, необходимо проинициализировать AviasalesSDK:

```java
  		AviasalesSDK.getInstance().init(this, new IdentificationData(TRAVEL_PAYOUTS_MARKER, TRAVEL_PAYOUTS_TOKEN)); 
```

Замените `TRAVEL_PAYOUTS_MARKER` и `TRAVEL_PAYOUTS_TOKEN` на ваш партнерский маркер и токен. Чтобы получить их, перейдите в раздел "[Разработчикам](https://www.travelpayouts.com/developers/api)" личного кабинета партнерской программы.

### Установка разрешений

Установите разрешения `INTERNET` и `ACCESS_NETWORK_STATE`, путем добавления элемента `<uses-permission>` в качестве вложения в элемент `<manifest>`. 

```xml
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

## Пример добавления шаблонного проекта Aviasales Template в приложение 

### Добавление `AviasalesFragment` в activity 

Добавьте в main activity, которая расположена в `activity_main.xml`, фрагмент `FrameLayout`:

```xml
 	<FrameLayout
		android:id="@+id/fragment_place"
		android:layout_width="match_parent"
		android:layout_height="match_parent"/>
```

Добавьте фрагмент в `MainActivity`:

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

### Добавление onBackPressed 

Для корректной работы `onBackPressed` между экранами Aviasales Template, добавьте следующее действие в `onBackPressed`: 

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


## Настройка

Для правильной настройки шаблонного проекта используйте тему AviasalesTemplateTheme, или унаследуйтесь от неё и измените её по своему усмотрению. Например:

```xml    
    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            ...
```

В файле `styles.xml` унаследуйте тему от AviasalesTemplateTheme:

```xml
	<style name="AppTheme" parent="AviasalesTemplateTheme">
            ...
	</style>
```

Чтобы изменить цвета вашего приложения, переопределите элементы `colorAsPrimary`, `colorAsPrimaryDark` и `colorAviasalesMain` в `colors.xml`

```xml
    <color name="colorAsPrimary">#3F51B5</color>
    <color name="colorAsPrimaryDark">#3F51B5</color>
    <color name="colorAviasalesMain">#3F51B5</color>

```

Так же вы можете изменить цвет фона приложения или цвет цены в выдаче:

```xml
	<color name="aviasales_results_background">@color/grey_background</color>
	<color name="aviasales_search_form_background">@color/white</color>
	<color name="aviasales_filters_background">@color/white</color>

	<color name="aviasales_select_airport_background">@color/white</color>
	<color name="aviasales_ticket_background">@color/grey_background</color>

	<color name="aviasales_results_card_color">@color/white</color>
	<color name="aviasales_price_color">@color/yellow_FDCC50</color>
```

Получите больше информации о [demo проекте](https://github.com/KosyanMedia/Aviasales-Android-SDK/tree/master/demo).


### Реклама Appodeal 
Так же вы можете добавить [рекламу Appodeal](http://www.appodeal.ru/) в ваше приложение и получать с неё прибыль

![][1]

Для того чтобы подключить Appodeal нужно просто добавить ещё одну maven-зависимость:

```gradle
dependencies {
    compile 'ru.aviasales.template:appodeallib:2.1.8'
}
```

И потом проинициализировать AppodealAds

```java
		AppodealAds ads = new AppodealAds(); 
		ads.setStartAdsEnabled(SHOW_ADS_ON_START); // реклама на старте (true/false)
		ads.setWaitingScreenAdsEnabled(SHOW_ADS_ON_WAITING_SCREEN); // реклама на экране ожидания (true/false)
		ads.setResultsAdsEnabled(SHOW_ADS_ON_SEARCH_RESULTS); // реклама в результатах (true/false)
		ads.init(this, APPODEAL_APP_KEY);  // appodeal ключ (id)
		AdsImplKeeper.getInstance().setCustomAdsInterfaceImpl(ads); // привязка рекламы к нашему проекту
```

Узнать больше про интеграцию рекламы Appodeal можно в [этом примере](https://github.com/KosyanMedia/Aviasales-Android-SDK/tree/master/ads_simple_demo)

### [Установка и настройка Aviasales Android SDK API](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Установка-и-настройка-Aviasales-Android-SDK-API)

###[Экраны шаблонного проекта](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/%D0%AD%D0%BA%D1%80%D0%B0%D0%BD%D1%8B-%D0%B2-%D1%81%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%B5-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE-%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0)

[1]: https://github.com/KosyanMedia/Aviasales-Android-SDK/tree/master/screenshots/Screenshot_ads1.png