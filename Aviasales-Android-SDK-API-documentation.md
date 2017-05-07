Aviasales SDK API is an .aar library which responsible for connection to Aviasales search engine. 

[Javadoc](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/index.html)

## Installation 

### Add gradle dependencies 

To add dependencies in the project use the gradle:

```gradle
repositories {
    maven { url 'http://android.aviasales.ru/repositories/' }
}

dependencies {
    compile 'ru.aviasales:aviasalesSdk:2.1.7-sdk'
}
```

### Initialization 

Before usage of sdk API it should be initialized 

```java
 AviasalesSDK.getInstance().init(this, new IdentificationData(TRAVEL_PAYOUTS_MARKER, TRAVEL_PAYOUTS_TOKEN));
```

Change TRAVEL_PAYOUTS_MARKER and TRAVEL_PAYOUTS_TOKEN to your marker and token params. You can get them at [Travelpayouts.com](https://www.travelpayouts.com/developers/455fbbe6dd6ae4aa9f9bf0e7b716a3dc):

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

		//originIata and destinationIata is Iatas of your current search 
		// dates are set in "yyyy-MM-dd" format (SearchParams.SEARCH_PARAMS_DATE_FORMAT)
		params.addSegment(originIata, destinationIata, departureDate);

		// this is return segment
		params.addSegment(destinationIata, originIata, returnDate);

		//set number of passengers
		params.setPassengers(new Passengers( 1, 0, 0 ); // number of adults, childrens, infants

		// trip class could be SearchParams.TRIP_CLASS_ECONOMY
		// or SearchParams.TRIP_CLASS_BUSINESS
		// or SearchParams.TRIP_CLASS_PREMIUM_ECONOMY
		params.setTripClass(tripClass);

		// pass application context
		params.setContext(context.getApplicationContext());
 ```

Aviasales SDK supports complex search with multiple segments. You can add up to 8 segments 
```java			
		params.addSegment("MOW", "LED", "2016-06-06");
		params.addSegment("LED", "BER", "2016-06-08");
		params.addSegment("BER", "ROM", "2016-06-10");
		params.addSegment("ROM", "MOW", "2016-06-12");
	});

```

Start ticket search:

```java			
		   AviasalesSDK.getInstance().startTicketsSearch(
                   searchParams, new SearchListener() {
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

	// Locale for searching . For now Aviasales supports ru, en, fr, de, it, es, th, pl, pt locales
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

	// proposal is ticket which user selected for purchase. List of proposals returns after successful search and stored in AviasalesSDK.getInstance.getSearchData().getProposals();
	// gateKey is gate ID of purchased proposal
	AviasalesSDK.getInstance().startBuyProcess(proposal, String gateKey,new BuyProcessListener() {
		... // Listener for response 
	});
```

### Search nearest airports

```java

	AviasalesSDK.getInstance().getNearestPlaces(java.lang.String locale, new OnNearestPlacesListener() {
		... // Listener for response 
	});
```

For more information see the [demo project](https://github.com/KosyanMedia/Aviasales-Android-SDK/tree/master/aviasalestemplate)

### Additional partner marker

In the app you can add additional affiliate marker. This is useful, for example, to monitor the actions of different users. To do this, initialize AviasalesSDK with IdentificationData constructor :
```java
		AviasalesSDK.getInstance().init(getApplicationContext(), new IdentificationData(TRAVEL_PAYOUTS_MARKER, YOUR_ADDITIONAL_MARKER, TRAVEL_PAYOUTS_TOKEN));
```
## Javadoc

Main class of API library is [AviasalesSDK.java](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/ru/aviasales/core/AviasalesSDK.html)

More information about API library you can get at [Aviasales API javadoc](http://kosyanmedia.github.io/Aviasales-Android-SDK/javadoc/index.html).

###[Aviasales SDK Template project](https://github.com/KosyanMedia/Aviasales-Android-SDK#installation)

###[Template project screens](https://github.com/KosyanMedia/Aviasales-Android-SDK/wiki/Template-project-screens)
