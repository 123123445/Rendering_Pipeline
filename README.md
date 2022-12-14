![Pipeline](https://user-images.githubusercontent.com/102730775/191171594-a35a8bd3-6db3-41a2-8209-6f5f6bec1b3d.png)
<h3>렌더링 파이프라인이란?</h3>
기하학적으로 3D 장면을 구성하고 가상의 카메라를 설정한 뒤에 모니터에 2D 표현을 만들어내는 과정을 렌더링 파이프라인이라 한다.

<h3>1.로컬 스페이스 (=모델링 스페이스)</h3>
로컬 스페이스는 우리가 물체의 삼각형 리스트를 정의하는 데 이용하는 좌표 시스템이다. 로컬 스페이스는 모델링 과정을 쉽고 단순하게 만들어준다

<h3>2.월드 스페이스</h3>
 로컬 스페이스로 구성한 다수의 모델들은 월드 좌표 시스템으로 옮겨 하나의 장면을 구성해야 한다. 로컬 스페이스의 물체들은 이동, 회전, 크기 변환 등을 포함하는 월드 변환이라는 작업을 거쳐 월드 스페이스로 옮겨진다, 변환 타입에 D3DTS_WORLD를 지정하고 IDirect3DDevice9::SetTransform 메서드를 호출하여 수행할 수 있다.
 
 <h3>3.뷰 스페이스</h3>
  월드 스페이스 내에서 기하물체와 카메라는 월드 좌표 시스템과 연계되어 정의한다. 그러나 카메라가 월드 내 임의의 위치나 방위를 가진다면 투영이나 그 밖의 작업이 어렵거나 비효율적이되는데, 이를 해결하기 위해 카메라를 월드 시스템의 원점으로 변환하고, 카메라가 양의 z축을 내려다 보도록 회전시키는 작업을 한다.
  
  <h3>4.후면 추려 내기 (=백 스페이스 컬링)</h3>
 폴리곤은 두 개의 면을 가지고 있다 화면에 보이는 일반적인 물체는 전면을 보여주며, 후면의 폴리곤은 보이지 않아야한다. 즉, 후면의 폴리곤을 처리에서 제거하는 작업을 후면 추려 내기라한다.
 
 <h3>5.조명</h3>
 광원은 월드 스페이스 내에 정의되지만, 뷰 스페이스 변환에 의해 뷰 스페이스로 변환 된다.
 
 <h3>6.클리핑</h3>
 (안보이는 부분을 잘라내 최적화 해주는거 같다)
 시야 볼륨 외부의 기하물체를 추려내는 과정을 클리핑(Clipping) 이라한다. 시야 절두체에서의 삼각형 위치는 다음과 같이 구분할 수 있다.

+ 완전한 내부 - 삼각형이 완전히 절두체 내부에 위치하면 그대로 보존되어 다음 단계로 진행한다.
+ 완전한 외부 - 삼각형이 완전히 절두체 와부에 위치하면 추려내어진다.
+ 부분적 내부 - 삼각형이 부분적으로 절두체 내부에 위치하면 삼각형을 두개의 부분으로 분리한다. 절두체 내부의 부분은 보존되며, 나머지는 추려내어진다.

<h3>7.투영</h3>
(화면에 표시하기위해 3d를 2d로 보이게 바꾸는 과정)
3D 장면의 2D 표면을 얻는 과정이 남아있다. 이와 같이 n 차원에서 n-1 차원을 얻는 과정을 투영이라 한다. 원근 투영을 이용하여 기하물체를 투사한다.

<h3>8.뷰포트 변환</h3>
(창모드처럼 좌표가 달라지는경우를 말하는것)
프로젝트의 윈도우의 좌표를 뷰 포트라 불리는 화면의 직사각형으로 변환하는 과정을 말한다. 게임에서의 뷰 포트는 직사각형의 전체 화면이 되지만, 윈도우 모드에서 실행하는 경우에는 클라이언트 영역이나 화면의 일부가 될 수도 있다.

<h3>9.레스터라이즈</h3>
 스크린 좌표로 버텍스들이 변환한 다음에는 2D 삼각형들의 리스트를 가지게 된다. 레스터라이즈 단계는 각각의 삼각형을 그리는데 필요한 픽셀컬러들을 계산한다.
