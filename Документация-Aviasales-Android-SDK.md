Шаблонный проект - это простой модульный проект, который использует Aviasales SDK, для поиска и бронирования дешевых авиабилетов.

## Установка SDK

### Добавление зависимостей

Чтобы добавить зависимости в проект, используйте gradle:

```gradle
repositories {
    maven { url 'http://android.aviasales.ru/repositories/' }
}

dependencies {
    compile 'ru.aviasales.template:aviasalesSdkTemplate:1.0.1'
}
```

### Добавление ключей Aviasales API в приложение

В strings.xml создайте строки `aviasales_marker` и `aviasales_api_token` с вашим партнерским маркером и токеном. Чтобы получить их, перейдите в раздел "[Разработчикам](https://www.travelpayouts.com/developers/api)" личного кабинета партнерской программы:

```xml
	<string name="aviasales_marker">74590</string>
	<string name="aviasales_api_token">9f16d617b9df8b2b6b5d0372711e9d6b</string>
```

Так же добавьте ключи в файлы `AndroidManifest.xml`, `ru.aviasales.marker` и `ru.aviasales.api_token` в качестве вложенных элементов в  `<application>` перед закрывающимся тегом `</application>`:
```xml
 <meta-data android:name="ru.aviasales.marker" android:value="@string/aviasales_marker"/>
 <meta-data android:name="ru.aviasales.api_token" android:value="@string/aviasales_api_token"/>
```

### Установка разрешений

Установите разрешения `INTERNET` и `ACCESS_NETWORK_STATE`, путем добавления элемента `<uses-permission>` в качестве вложения в элемент `<manifest>`. 

```xml
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

## Добавление шаблонного проекта в приложение 

### Добавление `AviasalesFragment` в activity 

Добавить в основной деятельности расположенной в `` activity_main.xml` фрагмент `FrameLayout`:

```xml
 	<FrameLayout
		android:id="@+id/fragment_place"
		android:layout_width="match_parent"
		android:layout_height="match_parent"/>
```

Добавить фрагмент в `MainActivity:

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

### Добавление onBackPressed 

Для корректной работы действия `onBackPressed` из раздела aviasales добавьте следующее действие в 'onBackPressed`: 

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

Для правильной настройки шаблонного проекта используйте элемент `AviasalesTemplateTheme`. Вы можете изменять его по своему усмотрению:

```xml    
    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            ...
```

И в `styles.xml`

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

### Шаблон поиска с тулбаром 

Если вы используете или планируете использовать тулбар в своем приложении, отключите actionbar в вашей теме: 

```xml
        <item name="windowActionBar">false</item>
        <item name="android:windowNoTitle">true</item>
```

и добавьте ваш тулбар вместо actionbar:

```java	
		mToolbar = (Toolbar) findViewById(R.id.toolbar_actionbar);
		setSupportActionBar(mToolbar);
```

Получите больше информации о [demo проекте](https://github.com/KosyanMedia/Aviasales-Android-SDK/tree/master/demo).

### [Aviasales Android API](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/%D0%94%D0%BE%D0%BA%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D1%8F-Aviasales-Android-SDK-API)
###[Экраны шаблонного проекта](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/%D0%AD%D0%BA%D1%80%D0%B0%D0%BD%D1%8B-%D0%B2-%D1%81%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%B5-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE-%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0)