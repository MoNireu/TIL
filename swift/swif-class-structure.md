#Class & Structure
* Class와 Struct는 swift의 근간을 이루는(객체지향) 매우 중요한 핵심 요소이다.
* Class와 Struct는 Member(Property / Method)를 포함한다.
	* Property : Struct와 Class 내부에서 정의된 변수나 상수.
	* Method : Struct와 Class 내부에 정의된 함수.

## Class VS Structure
### 공통점
	* Property 및 메소드 정의
	* 서브스크립트 정의
	* 초기화 블록 정의
	* 확장(extends) 구문
	* 프로토콜 구현
### 차이점(Class만 가능한 것들)
	* 상속
	* 타입 캐스팅
	* 소멸화 구문
	* 참조에 의한 전달(Struct는 값에 의한 전달) 

### Struct 사용 목적
* 서로 연관된 몇 개의 기본 데이터 타입 캡술화.
* 상속이 필요하지 않을때.
* 데이터 전달 방식이 값에 의한 전달일 경우 더 효율적일 때.
* 캡슐화된 원본 데이터 보존.

### 값 전달 방식
* Class : 참조에 의한 전달(Reference Type) 
* Struct : 값에 의한 전달(Value Type) 

#### Class - 참조에 의한 전달(Reference Type)
* 한 곳에서 수정할 경우 다른 곳에도 적용된다.
* 하나의 Class Instance는 오로지 하나의 변수/상수만이 참조할 수 있다.
* 하나의 Class Instance를 여러 변수나 상수, 또는 함수의 인자값에서 동시에 참조할 수 있다.

~~~swift
class VideoMode {
var interlaced = false
var frameRate = 0.0
var name: String?
}

func changeName(v: VideoMode) {
    v.name = "Function Video Instance"
}

let video = VideoMode()
video.name = "Original Video Instance"
print("video 인스턴스의 name 값은 \(video.name!)입니다.")
//video 인스턴스의 name 값은 Original Video Instance입니다.

let dvd = video
dvd.name = "DVD Video Instance"
print("video 인스턴스의 name 값은 \(video.name!)입니다.")
//video 인스턴스의 name 값은 DVD Video Instance입니다.

changeName(v: video)
print("video 인스턴스의 name 값은 \(video.name!)입니다.")
//video 인스턴스의 name 값은 Function Video Instance입니다.
~~~ 
##### 인스턴스 비교
* 참조 타입이기 때문에 같은 인스턴스 비교하는 연산자 활용
	* 동일 인스턴스 인지 비교 : ===
	* 동일 인스턴스가 아닌지 비교 : !==

동일 인스턴스 참조

~~~swift
class VideoMode {
var interlaced = false
var frameRate = 0.0
var name: String?
}

let video = VideoMode()
let dvd = video

if (video === dvd) {
    print("video와 DVD는 동일한 인스턴스를 참조하고 있습니다.")
} else {
    print("video와 DVD는 서로 다른 인스턴스를 참조하고 있습니다.")
}

// "video와 DVD는 동일한 인스턴스를 참조하고 있습니다."
~~~
서로 다른 인스턴스 참조

~~~swift
class VideoMode {
var interlaced = false
var frameRate = 0.0
var name: String?
}

let video = VideoMode()
let dvd = VideoMode()

if (video === dvd) {
    print("video와 DVD는 동일한 인스턴스를 참조하고 있습니다.")
} else {
    print("video와 DVD는 서로 다른 인스턴스를 참조하고 있습니다.")
}

// "video와 DVD는 서로 다른인스턴스를 참조하고 있습니다."
~~~


#### Struct - 값에 의한 전달(Value Type) 
* 모든 Struct Instance들은 상수나 변수에 할당될 때 복사됨.
* 따라서 변수에 대입된 기존의 Instance는 서로 독립적이다.
* Instance가 상수/변수 중 어느곳에 할당 되는가에 따라 Property값을 변경 여부가 달라진다.
	* 상수 할당 : Property 변경 불가
	* 변수 할당 : Property 변경 가능

~~~swift
struct Resolution {
    var width = 0
    var heigth = 0
}

let hd = Resolution(width: 1920, heigth: 1080)

var cinema = hd
cinema.width = 2048

print("hd 인스턴스의 width 값은 \(hd.width)입니다.")
//"hd 인스턴스의 width 값은 1920입니다.\n"

print("cinema 인스턴스의 width 값은 \(cinema.width)입니다.")
//"cinema 인스턴스의 width 값은 2048입니다."
~~~


## 인스턴스(Instance)
> 메모리의 공간을 할당 받는 객체

* Instance를 생성하여 Property에 접근한다.
* dot 문법(.)을 통해 Property에 접근할 수 있다.

## 초기화(Initialize)
* 스위프트에서 Optional로 설정되지 않은 모든 Property는 명시적으로 초기화해 주어야 한다.
* 초기화는 다음 방식으로 수행한다.
	* Property를 선언하면서 동시에 초기값을 지정.
	* 초기화 메소드 내에서 Property의 초기값을 지정.
	* 위 두 방법이 불가할 경우 Optional 타입으로 설정.

### 기본 초기화 구문(Initializer)
* 인스턴스만을 생성하는 초기화 구문이다.
* 어떠한 Property도 초기화하지 않는다.
* Class의 경우 모든 Property가 선언과 동시에 초기화되어 있을 경우에만 사용할 수 있다.

~~~swift
// 어떠한 인자도 받지 않고 단순히 Resolution 인스턴스만 생성
let defaultRes = Resolution()
~~~

### 멤버 와이즈 초기화 구문(Memberwise Initializer)
* Struct는 모든 Property의 값을 인자값으로 입력받아 초기화하는 멤버와이즈 초기화 구문을 제공한다.(Class는 제공하지 않음)
* 하나의 Property만 초기화하는 구문은 제공하지 않는다.

~~~swift
// width와 height를 매개변수로 하여 Resolution 인스턴스 생성
let defaultRes = Resolution(width: 1024, height: 768)
~~~

