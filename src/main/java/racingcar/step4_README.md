## **자동차 경주(4단계)**
##### Shortcuts
    * Command + N : Generate (Annotation, Override, Test method...)
                  : Generate Equals & HashCode (null여부 , field 정의)
    * Alt + Enter : auto Import, create Class (주의 : test 아닌 main dir)
                    @Test : method -> (1) class 생성 -> (2) method도 class내에 생성
    * Command + Alt + C : constant refactoring (magic number -> static constant)
    * Alt + Command + M : extract Method refactoring ***
    * Ctrl + Shift + R : Run
    * Shift + Fn + F6 : Rename var
    * Option + Command + L : alignment
    14:18, getMaxPosition도 메시지위임, int maxPosition 대신 Position obj, stream API
    생성자 this( ) overwrite확인 필요 , const 많은 것 구
#### [기능 요구사항]
* 각 자동차에 이름을 부여할 수 있다. 자동차 이름은 5자를 초과할 수 없다.
* 전진하는 자동차를 출력할 때 자동차 이름을 같이 출력한다.
* 자동차 이름은 쉼표(,)를 기준으로 구분한다.
* 자동차 경주 게임을 완료한 후 누가 우승했는지를 알려준다. 우승자는 한명 이상일 수 있다.


### 요구사항 분석 --> 구현할 기능 목록 
#####[]1. 자동차 이름 입력 ("," 기준) 후 자동차 객체 생성
    [V] (a) 참여자 이름 split하고 자동차 생성 
    [V] (b) 1자 이상, 5자 이하 정상 이름인지 확인
#####[]2. 자동차 이동/경주 횟수 및 거리 계산
    [] (c) 매 회 자동차 이동 (유무 / 거리) 계산
#####[]3. 자동차 이동 거리 따라 "이름 + " + "-" 생성 및 출력 
    [] (d) 자동차 이동 거리 따라 "-" 생성
#####[]4. 한 명 이상의 우승자 (가장 많은 이동거리) 찾아 이름 출력
    [] (e) 경주 참여 자동차 중 우승자(한 명 이상) 찾기 
    [] (f) 우승자(한 명 이상) 이름 출력하기     

##### TDD 도전 순서 정리 
##### 1. Util 성격, input-output 관계 명확한, static 중심 기능
        (b) 1자 이상, 5자 이하 정상 이름인지 확인
        (d) 자동차 이동 거리 따라 "-" 생성
##### 2. 외부에 종속이 덜한, Domain(model) 중심 기능
        (a) 참여자 이름 split하고 자동차 생성 : 'Car' 객체
        (e) 경주 참여 자동차 중 우승자(한 명 이상) 찾기 : 'Winners', 'RacingGame' 객체
##### 3. 외부의 종속(외부 API, 시스템 로직) -> 상위 node에 '위임' 통해 테스트 가능한 구조로 개선
        (c) 매 회 자동차 이동 (유무 / 거리) 계산
        (f) 우승자(한 명 이상) 이름 출력하기 

#### [실행 결과]
위 요구사항에 따라 3대의 자동차가 5번 움직였을 경우 프로그램을 실행한 결과는 다음과 같다.
    
    경주할 자동차 이름을 입력하세요(이름은 쉼표(,)를 기준으로 구분).
    pobi,crong,honux
    시도할 회수는 몇회인가요?
    5
    
    실행 결과
    pobi : -
    crong : -
    honux : -
    
    pobi : --
    crong : -
    honux : --
    
    pobi : ---
    crong : --
    honux : ---
    
    pobi : ----
    crong : ---
    honux : ----
    
    pobi : -----
    crong : ----
    honux : -----
    
    pobi : -----
    crong : ----
    honux : -----
    
    pobi, honux가 최종 우승했습니다.
    
#### [힌트]
* 규칙 1: 한 메서드에 오직 한 단계의 들여쓰기(indent)만 한다.
    * indent가 2이상인 메소드의 경우 메소드를 분리하면 indent를 줄일 수 있다.
    * else를 사용하지 않아 indent를 줄일 수 있다.
* 자동차 이름을 쉼표(,)를 기준으로 분리하려면 String 클래스의 split(",") 메소드를 활용한다.
* 사용자가 입력한 이름의 숫자 만큼 자동차 대수를 생성한다.
* 자동차는 자동차 이름과 위치 정보를 가지는 Car 객체를 추가해 구현한다.

#### [프로그래밍 요구사항]
* indent(인덴트, 들여쓰기) depth를 2를 넘지 않도록 구현한다. 1까지만 허용한다.
    [V] 규칙 1. 한 메서드에 오직 한 단계의 들여쓰기(indent)만 한다.
* 함수(또는 메소드)의 길이가 15라인을 넘어가지 않도록 구현한다.
* 모든 로직에 단위 테스트를 구현한다. 단, UI(System.out, System.in) 로직은 제외
* 자바 코드 컨벤션을 지키면서 프로그래밍한다.
* else 예약어를 쓰지 않는다.
    [V] 규칙 2. else 예약어를 쓰지 않는다. (if 예외상황 return 정값)
    
#### [리팩토링 Hint : RacingCar]
* 클래스 분리 (메서드 분리를 넘어)
    [V] 규칙 3. 모든 원시값과 문자열을 포장한다.
    [V] 규칙 8. 일급 콜렉션을 쓴다.