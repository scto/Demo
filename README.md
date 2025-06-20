# GithubCompose (Kotlin + Jetpack Compose + Navigation + MVI)

![Language](https://img.shields.io/github/languages/top/cortinico/kotlin-android-template?color=blue&logo=kotlin)

GithubCompose is a sample project that presents a modern approach to Android app development.

The project tries to combine popular Android tools and to demonstrate best development practices by utilizing up to date tech-stack like Compose, Kotlin Flow and Koin.

The sample app presents a modern Android application Architecture that is scalable and maintainable through a MVI.

## Description

* UI
    * [Compose](https://developer.android.com/jetpack/compose) declarative UI framework
    * [Material design](https://material.io/design)

* Tech/Tools
    * [Kotlin](https://kotlinlang.org/) 100% coverage
    * [Coroutines](https://kotlinlang.org/docs/reference/coroutines-overview.html) and [Flow](https://developer.android.com/kotlin/flow) for async operations
    * [Koin](https://insert-koin.io/) for dependency injection
    * [Jetpack](https://developer.android.com/jetpack)
        * [Compose](https://developer.android.com/jetpack/compose)
        * [Navigation](https://developer.android.com/topic/libraries/architecture/navigation/) for navigation between composables
        * [ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel) that stores, exposes and manages UI state
    * [Retrofit](https://square.github.io/retrofit/) for networking
    * [Coil](https://github.com/coil-kt/coil) for image loading

* Modern Architecture
    * Single activity architecture (with [Navigation component](https://developer.android.com/guide/navigation/navigation-getting-started)) that defines navigation graphs
    * MVI
    * [Android Architecture components](https://developer.android.com/topic/libraries/architecture) ([ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel), [Navigation](https://developer.android.com/jetpack/androidx/releases/navigation))
    * [Android KTX](https://developer.android.com/kotlin/ktx) - Jetpack Kotlin extensions
    
## Light and Dark mode
<img src="misc/demo_light_mode.gif" width="360" height="820"> <img src="misc/demo_dark_mode.gif" width="360" height="820">

| Dark Theme UI                            | Light Theme UI                              |
|-----------------------------------------|----------------------------------------------|
| <video src ='https://github.com/myofficework000/MVI-JetpackCompose-Github/assets/51234843/cbcd4ac6-cca1-44ab-a55d-c8095f775ee1' width=200/> | <video src ='https://github.com/myofficework000/MVI-JetpackCompose-Github/assets/51234843/5728e268-be26-44fb-81d0-a27663aea07f' width=200/>


## Architecture
The project is layered traditionally with a View, Presentation, Model separation and presents a MVI inspired from [Yusuf Ceylan's architecture](https://proandroiddev.com/mvi-architecture-with-kotlin-flows-and-channels-d36820b2028d) but adapted to Compose.

Architecture layers:
* View - Composable screens that consume state, apply effects and delegate events.
* ViewModel - [AAC ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel) that manages and reduces the state of the corresponding screen. Additionally, it intercepts UI events and produces side-effects. The ViewModel lifecycle scope is tied to the corresponding screen composable.
* Model - Repository classes that retrieve data. In a clean architecture context, one should use use-cases that tap into repositories.

![](https://i.imgur.com/UXwFbmv.png)

There are a three core components described:
* **State** - data class that holds the state content of the corresponding screen e.g. list of `User`, loading status etc. The state is exposed as a Compose runtime `MutableState` object from that perfectly matches the use-case of receiving continuous updates with initial value.

* **Event** - plain object that is sent through callbacks from the UI to the presentation layer. Events should reflect UI events caused by the user. Event updates are exposed as a `MutableSharedFlow` type which is similar to `StateFlow` and that behaves as in the absence of a subscriber, any posted event will be immediately dropped.

* **Effect** - plain object that signals one-time side-effect actions that should impact the UI e.g. triggering a navigation action, showing a Toast, SnackBar etc. Effects are exposed as `ChannelFlow` which behave as in each event is delivered to a single subscriber. An attempt to post an event without subscribers will suspend as soon as the channel buffer becomes full, waiting for a subscriber to appear.

Every screen/flow defines its own contract class that states all corresponding core components described above: state content, events and effects.
