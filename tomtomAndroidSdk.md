author:            Mariusz Saramak
summary:           TomTom Maps SDK Introduction
id:                24242
categories:        sdk
environments:      android
status:            draft
feedback link:     github.com/mariopce
analytics account: 0

Formatting help:
https://github.com/googlecodelabs/tools/tree/master/claat/parser/md

# TomTom Android Maps SDK

## Introduction
Duration: 0:05

### TomTom Services and SDK

Maps SDK for Android is a convenient Software Development Kit that helps you developers make the most out of the TomTom Online Services in your mobile application, without the complexity of bare REST API calls. Optimized for Android applications, the Maps SDK for Android allows you to easily configure and deploy different location services inside of your Android application.

Maps SDK for Android comes as modules so that you can use just the features you need:

1. Online Maps – display and interact with beautiful maps in your application using vector tiles, raster tiles, visualize traffic incidents and/or traffic flow on top of a map
2. Online Search – search for an address, for a Point of Interest or a combination of both. Includes auto-completion and correction for the best user experience
3. Online Routing – use the industry’s leading routing engine to calculate routes with advanced parameters such as traffic avoidance, eco routes, waypoint optimization, reachable range, time to leave, etc.

Maps SDK for Android comes with the documentation and examples for the key features of map, search and routing as well as real code samples in Maps SDK Examples app. Use this source code in your own app to speed up development and get to market faster!

Positive
: TomTom services: 2500 requests/per day are free

### What you'll need

1. Android Studio
2. JDK - Java Developer Kid

## Getting Started
Duration: 0:10

### Get your api key from developer.tomtom.com

Go to developer.tomtom.com and register. Log in and go to your dashboard.

![dashboard](https://cdn-images-1.medium.com/max/1200/1*cuJUdYj_GSdST8x4j_tyXw.png)

Press add a new App, choose a name and select product which you will use

![newapp](https://cdn-images-1.medium.com/max/800/1*3not_Sf1NyGmcdcXbDs9EA.png)

### Request an evaluation API key to access the underlying services

On your dashboard is a new app, please select the app to expand product and check key. The most important is Customer API Key ( 2500 request/per day are for free) This key is needed for sdk please copy this key to clipboard because it is needed in next steps.

Now you are ready to create a new application and display a map.

Positive
: If you forgot your key, you can sign in again to developer.tomtom.com and get a key from dashboard

## Create new application
Duration: 0:15

### Android studio
Run android studio and start new project

![newstudioapp](https://cdn-images-1.medium.com/max/800/1*BA8DclefaOkTFgWZbL5WNQ.png)

Create new android project — kotlin support

### Empty activity — with default names for layout and activity

![newstudioapp](https://cdn-images-1.medium.com/max/800/1*bQHNqUAx5IOagPHi6Fs-pg.png)

add to root/build.gradle a maven tomtom repository.

``` gradle
maven {
    url 'https://maven.tomtom.com:8443/nexus/content/repositories/releases/'
}
```

### In AndroidManifest.xml add your key OnlineMaps.Key:

![addkey](https://cdn-images-1.medium.com/max/800/1*XxHQXuMHhJ8d2VRnJZF2Qw.png)

### Dependency to app/build.gradle
``` gradle
implementation("com.tomtom.online:sdk-maps:2.+@aar"){
    transitive = true;
}
```

Log a message when the element is removed. Remove it by typing `$('my-element').remove()` in your console.
### Multidex
in android section please enable multidex ( it is simple when minSDKVersion is 21 or higher) _multiDexEnabled true_
``` gradle
android {
    defaultConfig {
        ...
        minSdkVersion 21
        targetSdkVersion 26
        multiDexEnabled true
    }
    ...
}
```

Negative
: When you use minSdkVersion lower then 21 then check link below

[multidex support](https://developer.android.com/studio/build/multidex.html)

## Display map
Duration: 0:15

### layout

Add a map to layout `main_layout.xml`:

``` xml
<com.tomtom.online.sdk.map.MapView
    android:id="@+id/map_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent">

</com.tomtom.online.sdk.map.MapView>
```

### pass lifecycle events to mapview

``` kotlin
class MainActivity : AppCompatActivity() {
    lateinit var mapView: MapView
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        mapView = findViewById<MapView>(R.id.map_view);
    }

    override fun onResume() {
        super.onResume()
        mapView.onResume()
    }

    override fun onPause() {
        mapView.onPause()
        super.onPause()

    }
}
```

## Map manipulation
Duration: 0:10
### Center map on Amsterdam

You need register on map ready callback, and then all operation you should make on tomtomMap object.

``` kotlin
mapView = findViewById<MapView>(R.id.map_view);
mapView.addOnMapReadyCallback { tomtomMap ->
    val amsterdam = LatLng(52.36,4.88)
    val zoomLevel = 12
    tomtomMap.centerOn(amsterdam, zoomLevel)
}
```

### Add marker

Adding marker is similar, register OnMapReadyCallback and when map is ready you can
add a marker.

``` kotlin
mapView = findViewById<MapView>(R.id.map_view);
mapView.addOnMapReadyCallback { tomtomMap ->
    val amsterdam = LatLng(52.36,4.88)
    tomtomMap.addMarker(MarkerBuilder(amsterdam))
}
```

## Enable logs
Duration: 0:03

Enable timber.

``` kotlin
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        //enable logs
        Timber.plant(Timber.DebugTree())
```
