# Scene Delegate
iOS 13버전 및 xCode 11로 업데이트 되면서 프로젝트 생성시 기본적으로 생성되는 `AppDelegate.swift`와 `ViewController.swift`이외에 `SceneDelegate.swift`가 추가적으로 생성된다. SceneDelegate는 기본적으로 iPad와 Mac에서 Multi-Window 환경을 지원하기 위해서 만들어졌다. Apple은 기존 Application LifeCycle의 전반을 담당하던 AppDelegate의 역할들 중 일부를 SceneDelegate로 분리했다. SceneDelegate는 무엇이며 어떻게 사용해야하는지 알아보자.

## App Delegate의 역할
새로 생긴 Scene Delegate를 이해하기 위해서는 기존 App Delegate가 어떻게 달라졌는지 이해가 필요하다.

기존 App Delegate의 주요 Method를 살펴보자.

* `func application(_:didFinishLaunchingWithOptions launchOptions: ) -> Bool`
* `func application(_:configurationForConnecting:options:) -> UISceneConfiguration `
* `func application(_:didDiscardSceneSessions:)`


### `func application(_:didFinishLaunchingWithOptions launchOptions: ) -> Bool`
* Application 최초 실행시 호출되는 Method이다.
* UIObject를 생성 및 설정하고, UIViewController를 할당하여 화면에 띄울 수 있었다.

* 하지만 이제는 Scene Delegate의 UISceneSession 때문에 AppDelegate는 더이상 single-window object를 관리하지 않게 되었다.

### `func application(_:configurationForConnecting:options:) -> UISceneConfiguration `
* Application이 새로운 Scene 및 Window를 display하려 할 때 호출된다.
* Application 최초 실행시 호출되는 Methon가 아니다.

### `func application(_:didDiscardSceneSessions:)`
* 사용자가 Multitask 창에서 Application을 종료하는 등 Application이 종료될 때 호출된다.



## SceneDelegate의 역할
AppDelegate가 Application Life-Cycle을 책임진다면, SceneDelegate는 Scene, Window등 어떠한 화면이 출력되는지에 관여한다.

다음은 SceneDelegate의 Methods이다.

* `scene(_:willConnectTo:options:)`
* `sceneWillEnterForeground(_:)`
* `sceneDidBecomActive(_:)`
* `sceneWillResignActive(_:)`
* `sceneDidEnterBackground(_:)`
* `sceneDidDisconnect(_:)`


### `scene(_:willConnectTo:options:)`
* 호출 시 초기 Content View 화면, 새로운 UIWindow를 생성하고, Window의 rootViewController를 설정한다. 
* Application의 연결이 끊어졌을 때, UI 화면을 다시 불러오기 위해 호출 되기도 한다.

### `sceneWillEnterForeground(_:)`
* Backgound에 있던 Application이 Foregound로 올라와서 화면에 출력될 때 호출된다.

### `sceneDidBecomActive(_:)`
* `sceneWillEnterForeground(_:)`가 호출된 이후에 호출된다.
* 화면이 준비되었다는 의미이다.

### `sceneWillResignActive(_:)`
* 화면이 Background로 이동할 때 호출된다.

### `sceneDidEnterBackground(_:)`
* 화면이 Background로 이동 완료 되었을 때 호출된다.

### `sceneDidDisconnect(_:)`
* iOS의 resource 관리 등의 이유로 Scene이 메모리에서 정리할 때 호출된다.
* Scene이 다시 Foreground로 올라와 화면상에 출력 될 때를 대비하여 어떠한 데이터를 보존하고 어떠한 데이터를 해제할지 고려해야한다.
