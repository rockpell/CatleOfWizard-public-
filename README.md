# CatleOfWizard-public
## 개요
마법사가 미지의 도시로 불시착하고 플레이어를 공격하는 적들을 처리하고 도시를 탈출하는 VR게임입니다.

VR의 양손 컨트롤러를 이용하여 마법을 생성하는것이 이 게임의 주요 컨텐츠입니다.

AirSig라는 gesture 일치도를 보여주는 라이브러리를 이용하여 특정 패턴을 입력하면 마법을 사용할 수 있는 기능이 있습니다.

이 게임의 스테이지는 마법을 연습하고 게임의 조작법을 알려주는 튜토리얼 스테이지, 적이 나타나는 도시 스테이지로 구성되어 있습니다.

도시 스테이지에서 빛기둥이 위치한 곳 아래에 있는 텔레포트 게이트에 도착하면 게임이 클리어됩니다.

2019학년 졸업작품으로 개발한 프로그램입니다.

이 저장소는 3d모델링과 이펙트의 라이센스 문제로 스크립트만을 별도로 추출한 프로젝트입니다.

## 조작법
플레이어 이동: 오른쪽 컨트롤러 터치패드

마법 패턴 그리기: 오른쪽 컨트롤러 트리거

텔레포트: 왼쪽 컨트롤러 터치패드

기본 공격: 왼쪽 컨트롤러 트리거

도움말: 컨트롤러(왼쪽, 오른쪽) 메뉴 버튼

## 개발환경
Windows10(64bit)

## 작성언어
C#(Unity)

## 하드웨어
HTC VIVE

## 플랫폼
Windows10에 최적화되어 있습니다.

## 사용 IDE
Unity 2018.1.6f

Visual Studio 2017

## 소개 영상
<https://youtu.be/em7hyR5KgKo>

## 사용 디자인 패턴
- MVC
- 싱글톤 패턴

## 주요 클래스 설명
### ViveInputManager
SteamVR 플러그인을 이용하여 VR 컨트롤러의 입력을 관리합니다.

오른쪽 트리거 버튼을 입력하면 마법 패턴 그리기 및 경로 입력, 트리거 버튼을 놓았을 때는 입력한 경로와 일치하는 패턴이 있을 경우 마법 사용이 가능하게 설정하는 기능이 있습니다.

오른쪽 그립버튼을 입력하면 사용가능한 마법이 있을 경우 마법이 컨트롤러의 전면 방향으로 날아갑니다.

오른쪽 터치패드를 입력하면 PlayerController의 SimleMove 함수를 이용하여 플레이어를 이동합니다.

왼쪽 트리거 버튼을 입력하면 기본 마법을 플레이어의 왼손에 생성하고 ViveInputController의 GrabObject 함수를 이용하여 왼손에 붙어있도록 구현하였고, 트리거 버튼을 놓았을 때는 마법이 왼손에서 떨어집니다.

왼쪽 터치패드를 입력하면 컨트롤러가 향하는 방향으로 텔레포트 마크가 나타나고 입력을 중단하면 Teleport함수를 이용하여 플레이어를 순간이동시킵니다.
양쪽 컨트롤러의 메뉴 버튼을 이용하면 컨트롤러의 상단에 나타나는 도움말을 열고 닫을 수 있습니다.

### ViveInputController
FixedJoint 컴포넌트를 이용하여 원하는 오브젝트를 컨트롤러에 붙이고 떼어내는 기능을 관리합니다.

### PlayerController
플레이어의 이동, 걷는 소리, 데미지 이펙트를 관리합니다.

### MagicManager
AirSig 라이브러리에서 넘겨주는 일치된 패턴의 이름과 matchScore를 이용하여 일치되는 마법을 사용가능하게 하는 ActMagic함수를 가지고 있습니다.

사용 가능한 마법이 있을 경우 마법을 생성하여 날리는 ShootMagic를 가지고 있습니다.

### GuideImageController
ActGuideImage 함수를 통해 사용 가능한 마법이 생겼을 때 해당 마법이 무엇인지를 알려주는 이미지를 보여줍니다.

ActGuideImage 함수는 MagicManager의 ActMagic함수에서 실행됩니다.

### GameManager
스테이지 관리 및 게임의 시작을 관리합니다.

### CarController
지정된 경로를 따라 이동합니다.

목적지에 도달하면 CarManager의 EnqueueCar 함수를 이용하여 오브젝트를 비활성화 합니다.

이동시에 연기를 남기는 이펙트를 관리합니다.

### CarManager
3군데의 자동차 생성위치에 코루틴을 이용하여 일정 주기로 자동차를 생성합니다.

자동차 생성 초기에 carIndex를 랜덤으로 설정하고 inactiveCarQueue가 비어있다면 carIndex값에 따라 다른 종류의 자동차를 생성합니다.

inactiveCarQueue에 오브젝트가 있다면 DequeueCar 함수를 이용하여 비활성화된 오브젝트를 활성화합니다.

자동차 생성 및 오브젝트 활성화는 OptimizeCreateCar 함수를 통해 이루어집니다.

OptimizeCreateCar 함수는 isCreateRunnig bool 변수와 WaitUntil 코루틴을 이용하여 이미 실행 중인 OptimizeCreateCar 함수가 있다면 대기하도록 합니다.(뮤텍스)
