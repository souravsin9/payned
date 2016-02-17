Aviasales SDK API находится в .aar библиотеке, которая отвечает за подключение к движку поиска билетов от Aviasales. 

[Javadoc](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/index.html)

## Установка

### Добавление зависимостей 

Чтобы добавить зависимости в проект, используйте gradle:

```gradle
repositories {
    maven { url 'http://android.aviasales.ru/repositories/' }
}

dependencies {
    compile 'ru.aviasales:aviasalesSdk:2.0.8-sdk'
}
```

### Инициализация

Перед тем как использовать SDK API его необходимо проинициализировать: 

```java
    AviasalesSDK.getInstance().init(this, new IdentificationData(TRAVEL_PAYOUTS_MARKER, TRAVEL_PAYOUTS_TOKEN));

```
Замените TRAVEL_PAYOUTS_MARKER и TRAVEL_PAYOUTS_TOKEN на ваш партнерский маркер и токен. Чтобы получить их, перейдите в раздел "[Разработчикам](https://www.travelpayouts.com/developers/api)" личного кабинета партнерской программы.

### Установка разрешений

Установите разрешения INTERNET и ACCESS_NETWORK_STATE. Для этого добавьте элемент <uses-permission> в качестве вложения в элемент <manifest>.

```xml
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

## Использование

### Поиск билетов 

Чтобы в приложении работал поиск билетов, создайте SearchParams:

```java
		SearchParams params = new SearchParams();

		//originIata и destinationIata это IATA коды текущего поиска 
		// даты должны быть в формате "yyyy-MM-dd" (SearchParams.SEARCH_PARAMS_DATE_FORMAT)
		params.addSegment(originIata, destinationIata, departureDate);

		// сегмент возвращения
		params.addSegment(destinationIata, originIata, returnDate);

		//устанавливаем количество пассажиров
		params.setPassengers(new Passengers( 1, 0, 0 ); // количество взрослых, детей, младенцев соответственно

		// класс может быть эконом классом - SearchParams.TRIP_CLASS_ECONOMY
		// или бизнес классом SearchParams.TRIP_CLASS_BUSINESS
		// или премиум экономом SearchParams.TRIP_CLASS_PREMIUM_ECONOMY
		params.setTripClass(tripClass);

		// передаем контекст приложения
		params.setContext(context.getApplicationContext());
 ```

Aviasales SDK поддерживает сложный поиск. Можно задать до 8-ми поисковых сегментов
  
```java			
		params.addSegment("MOW", "LED", "2016-06-06");
		params.addSegment("LED", "BER", "2016-06-08");
		params.addSegment("BER", "ROM", "2016-06-10");
		params.addSegment("ROM", "MOW", "2016-06-12");
	});

```

Запуск поиска билетов:

```java			
		   AviasalesSDK.getInstance().startTicketsSearch(
                   searchParams, new SearchListener() {
		... // колбэк ответа
	});

```

### Поиск мест

Создайте SearchByNameParams: 

```java
	SearchByNameParams params = new SearchByNameParams();
	//Текст поиска
	params.setName(searchText);
	params.setContext(getActivity());

	// Язык поиска. На данный момент aviasales поддерживает следующие языки :ru, en, fr, de, it, es, th, pl, pt.
	params.setLocale("en");
```

Запуск поиска мест:
 
```java
		AviasalesSDK.getInstance().startPlacesSearch(params, new OnSearchPlacesListener() {
		... // колбэк ответа 
	});
```

### Покупка билетов

Запуск процесса покупки билетов:

```java
	// proposal это объект билета, который был выбран для покупки. Список proposals возвращается после успешного поиска и хранится в AviasalesSDK.getInstance.getSearchData().getProposals();
	// gateKey это ID гейта купленного proposal
	AviasalesSDK.getInstance().startBuyProcess(proposal, String gateKey,new BuyProcessListener() {
		... // колбэк ответа
	});
```

### Поиск ближайших аэропортов

```java

	AviasalesSDK.getInstance().getNearestPlaces(java.lang.String locale, new OnNearestPlacesListener() {
		... // колбэк ответа
	});
```

### Использование дополнительного маркера

В приложении можно использовать дополнительный маркер. Это может пригодиться для отслеживания действий разных пользователей. Для этого необходимо при старте приложения инициализировать AviasalesSDK со следующим конструктором IdentificationData: 

```java
		AviasalesSDK.getInstance().init(getApplicationContext(), new IdentificationData(TRAVEL_PAYOUTS_MARKER, YOUR_ADDITIONAL_MARKER, TRAVEL_PAYOUTS_TOKEN));
```

## Javadoc

Основной класс API библиотеки - это [AviasalesSDK.java](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/ru/aviasales/core/AviasalesSDK.html)

Больше информации о библиотеке API вы можете получить в [Aviasales API javadoc](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/index.html).

###[Шаблонный проект Aviasales Android SDK](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Шаблонный-проект-Aviasales-Android-SDK)

###[Экраны в составе шаблонного проекта](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/%D0%AD%D0%BA%D1%80%D0%B0%D0%BD%D1%8B-%D0%B2-%D1%81%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%B5-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE-%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0)