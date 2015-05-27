Aviasales SDK API находится в .aar библиотеке которая отвечает за подключение к движку поиска билетов от Aviasales. 

## Установка

### Добавление зависимостей 

Чтобы добавить зависимости в проект, используйте gradle:

```gradle
repositories {
    maven { url 'http://android.aviasales.ru/repositories/' }
}

dependencies {
    compile 'ru.aviasales:aviasalesSdk:1.0.1'
}
```

### Инициализация

Перед тем как использовать SDK API его необходимо проинициализировать 

```java
    AviasalesSDK.getInstance().init(this);

```

### Добавление ключей Aviasales API в приложение

В strings.xml создайте строки aviasales_marker и aviasales_api_token с вашим партнерским маркером и токеном. Чтобы получить их, перейдите в раздел "Разработчикам" личного кабинета партнерской программы:

```xml
	<string name="aviasales_marker">74590</string>
	<string name="aviasales_api_token">9f16d617b9df8b2b6b5d0372711e9d6b</string>
```

Так же добавьте ключи в файлы AndroidManifest.xml, ru.aviasales.marker и ru.aviasales.api_token в качестве вложенных элементов в <application> перед закрывающимся тегом </application>:

```xml
 <meta-data android:name="ru.aviasales.marker" android:value="@string/aviasales_marker"/>
 <meta-data android:name="ru.aviasales.api_token" android:value="@string/aviasales_api_token"/>
```

### Установка разрешений

Установите разрешения INTERNET и ACCESS_NETWORK_STATE, путем добавления элемента <uses-permission> в качестве вложения в элемент <manifest>.

```xml
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

## Использование

### Поиск билетов 

Для запуска поиска билетов создайте SearchParams

```java
		SearchParams params = new SearchParams();
		params.setOriginIata(originIata);  // origin iata string ( for example LON)
		params.setDestinationIata(destinationIata); // destination iata string ( BER )

		// depart & return date are seting in "yyyy-MM-dd" format (SearchParams.SEARCH_PARAMS_DATE_FORMAT)
		params.setDepartDate(departDate); 
		params.setReturnDate(returnDate);// if one-way ticket should be null

		//number of passengers
		params.setAdults(adults); 
		params.setChildren(children);
		params.setInfants(infants);

		// trip class could be SearchParams.TRIP_CLASS_ECONOMY
		// or SearchParams.TRIP_CLASS_BUSINESS
		// or SearchParams.TRIP_CLASS_PREMIUM_ECONOMY
		params.setTripClass(tripClass);

		// set possiple stop over or nonstop
		params.setDirect(SearchParams.DIRECT_STOP_OVER);

		// these parameters should not be changed
		params.setRange(SearchParams.RANGE_EXACT);
		params.setEnableApiAuth(true);
		params.setPreinitializeFilters(true);

		// pass application context
		params.setContext(context.getApplicationContext());
 ```

Запуск поиска билетов:

```java			
		   AviasalesSDK.getInstance().startTicketsSearch(
                   searchParams, new OnTicketsSearchListener() {
		... // Listener for response 
	});

```

### Поиск мест

Создайте SearchByNameParams 

```java
	SearchByNameParams params = new SearchByNameParams();
	//Set search text
	params.setName(searchText);
	params.setContext(getActivity());

	// Locale for searching . For now aviasales supports ru, en, fr, de, it, es, th, pl, pt locales
	params.setLocale("en");
```

Запуск поиска мест:
 
```java
		AviasalesSDK.getInstance().startPlacesSearch(params, new OnSearchPlacesListener() {
		... // Listener for response 
	});
```

### Покупка билетов

Запуск процесса покупки билетов:

```java
	AviasalesSDK.getInstance().startBuyProcess(ticketData, String gateKey,new OnBuyProcessListener() {
		... // Listener for response 
	});
```


## Javadoc

Основной класс API библиотеки это [AviasalesSDK.java](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/ru/aviasales/core/AviasalesSDK.html)

Больше информации о библиотеке API вы можете получить в [Aviasales API javadoc](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/index.html).

###[Экраны в составе шаблонного проекта](https://github.com/KosyanMedia/aviasales-android-template/wiki/%D0%AD%D0%BA%D1%80%D0%B0%D0%BD%D1%8B-%D0%B2-%D1%81%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%B5-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE-%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0)