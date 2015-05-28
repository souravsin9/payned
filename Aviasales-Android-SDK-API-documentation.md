Aviasales SDK API is an .aar library which responsible for connection to Aviasales search engine. 

## Installation 

### Add gradle dependencies 

To add dependencies in the project use the gradle:

```gradle
repositories {
    maven { url 'http://android.aviasales.ru/repositories/' }
}

dependencies {
    compile 'ru.aviasales:aviasalesSdk:1.0.1'
}
```

### Initialization 

Before usage of sdk API it should be initialized 

```java
    AviasalesSDK.getInstance().init(this);

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

## Usage 

### Ticket search 

To start ticket search create `SearchParams`:

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

Start ticket search:

```java			
		   AviasalesSDK.getInstance().startTicketsSearch(
                   searchParams, new OnTicketsSearchListener() {
		... // Listener for response 
	});

```

### Finding places

Create `SearchByNameParams`:

```java
	SearchByNameParams params = new SearchByNameParams();
	//Set search text
	params.setName(searchText);
	params.setContext(getActivity());

	// Locale for searching . For now aviasales supports ru, en, fr, de, it, es, th, pl, pt locales
	params.setLocale("en");
```

Start places search 
```java
		AviasalesSDK.getInstance().startPlacesSearch(params, new OnSearchPlacesListener() {
		... // Listener for response 
	});
```

### Buying ticket

Start buy process:
```java
	AviasalesSDK.getInstance().startBuyProcess(ticketData, String gateKey,new OnBuyProcessListener() {
		... // Listener for response 
	});
```

For more information see [demo project](https://github.com/KosyanMedia/Aviasales-Android-SDK/tree/master/aviasalestemplate)

## Javadoc

Main class of API library is [AviasalesSDK.java](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/ru/aviasales/core/AviasalesSDK.html)

More information about API library you can get at  [ Aviasales API javadoc](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/index.html).

###[Aviasales SDK Documentation](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Aviasales-Android-SDK-Documentation)

###[Template project screens](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Template-project-screens)