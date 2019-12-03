#Property List
* 객체 직렬화를 위한 XML 형식의 파일.
* 단순한 데이터를 XML 포맷에 맞추어 (Key-Value)형식의 Dictionary 형식으로 저장하는 것.
* 데이터의 타입을 추상화하여 저장.
* Primitive Data Type 또는 Reference Data Type으로 저장.
	* Primitive Data Type :
		* String, Int, Float, Double, Bool, ...
	* Reference Data Type :
		* NSString, NSNumber, NSDate, NSData, CGString, CGNumber, CGDate, CFData, ...


## User Defaults
* iOS 기본 저장소를 이용할 수 있는 객체.
* 기본 저장소 plist의 경우 < App ID >.plist 형식으로 생성됨.
* Singleton Pattern으로 설계 됨.
* 동시성 문제
	* 하나의 공유되는 자원에 여러 객체가 동시에 접근하며 발생.
	* 이에 대응하기 위해 Blocking Algorithm 및 Thread-Safe Designed 되었음.

### API
#### 데이터 쓰기
```
set(_value:Any?, forKey:String)
set(_value:Bool, forKey:String)
set(_value:Double, forKey:String)
set(_value:Float, forKey:String)
set(_value:Int, forKey:String)
.
.
.
```

위와 같이 Method Overrloading 되어있기 때문에 `set(_:forKey:)`만 사용하면 됨.

예시

```
let plist = UserDefaults.standard

plist.set("홍길동", forKey: "name")
plist.set(24, forKey: "age")
plist.set("남", forKey: "sex")
```

#### 데이터 읽기
```
string(forKey:String)
integer(forKey:String)
object(forKey:String)
.
.
.
```
예시

```
let plist = UserDefaults.standard

let name = plist.string(forKey: "name")
let age = plist.integer(forKey: "age")
let sex = plist.object(forKey: "sex") as? NSString
```

#### 데이터 동기화
`plist.synchronize()`사용

## Custom plist
* `UserDefaults`와 같이 전용 메소드가 없음.
* 따라서 Dictionary 객체를 이용하여 데이터 정의 후, 파일에 직접 기록. 파일을 읽을 때도 파일을 읽어들인 후, 딕셔너리 객체 타입으로 변환해야함.

### 데이터 처리 순서
1. plist 파일을 읽어와 Dictioinary 객체에 담는다.
2. Dictionary 객체에 필요한 값을 추가하거나 수정한다.
3. Dictionary 객체를 파일에 기록한다.

예시

```
import Foundation

// 1단계: data.plist 파일을 읽어온다.
let paths = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)

let path = paths[0] as NSString
let plist = path.strings(byAppendingPaths: ["data.plist"]).first
let data = NSMutableDictionary(contentsOfFile: plist!)

// 2단계:
// 2-1) 저장된 데이터를 읽어온다.
let name = data?.value(forKey: "name") as? String
let age = data?.value(forKey: "age") as? Int

// 2-2) 읽어온 데이터를 출력한다.
if let _name = name {print(_name)}
if let _age = age {print(_age)}

// 2-3) 값을 입력 또는 수정한다.
data?.setValue("홍길동", forKey: "name")
data?.setValue(32, forKey: "age")
data?.setValue("남", forKey: "sex")

// 3단계: Dictionary 객체를 파일에 저장한다.
data?.write(toFile: plist!, atomically: true)
```

## UserDefaults vs Custom plist
### UserDefaults를 사용하는 경우
* 앱 전체에서 사용되는 공통 데이터일 경우
* plist로 분리하기엔 너무 데이터 양이 적을 경우
* 비교적 간단하고 작은 데이터

### Custom plist를 사용하는 경우
* 일부 기능에 국한된 데이터
* 데이터 양이 많을 경우
* 비슷한 형식의 데이터 그룹이 반복되는 경우

예시

|     key    |   A.plist   |   B.plist   |   C.plist   |
|:----------:|:-----------:|:-----------:|:-----------:|
| account    |      a      |      b      |      c      |
| email      | a@swift.com | b@swift.com | c@swift.com |
| updateFlag |     true    |    flase    |     true    |