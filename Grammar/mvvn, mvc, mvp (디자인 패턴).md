# mvvn, mvc, mvp (디자인 패턴)

------

- mvc

  mvc(model, view, controller)

  - **Model** :

  Data(데이터) + Status(상태) + Logic(로직)을 담당

  - **View** :

  Model을 표현하고 책임지는 객체

  - **Controller** :

  Model과 View를 연결해주고 관리하는 객체

  장점

  • Model과 View를 확실하게 분리시켜준다.

  단점

  • Controller가 커지고 시간이 지나면 유지보수가 어려움

- mvvn

  mvvn(model, view, view model)

  - **Model :**

  MVC, MVP에서의 Model와 같음

  - **View :**

  View Controller가 View의 역할을 함

  - **View Model :**

  Model을 소유하고 모델을 직접 갱신한다. 갱신에 대한 이벤트 및 액션을 View에 바인딩하여 전달한다.

  장점

  • MVP보다 깔끔하며 코딩 양이 적다.

  • View Model이 View에 대해서 전혀 모르며 테스트가 용이

  단점

  • 간단한 앱의 경우 MVC가 오히려 관리가 편하고 코드가 적다.

  • 기본 MVP의 View보다 MVVM의 View는 책임의 범위가 더 크다.

- mvp

  mvp(model, view, present)

  - **Model :**

  Data(데이터) + Status(상태) + Logic(로직)을 담당

  - **(Passive) View :**

  Presenter를 소유하며 액션을 보내주는 객체

  - **Presenter :**

  Model을 소유하고 모델이 갱신되면 View 에 전달

  장점

  • 전반적으로 MVC보다 깔끔한 코딩이 가능

  • Model 및 Presenter에 거의 책임을 분리하여 View가 간결해짐

  단점

  • Presenter가 커지면 기존 MVC 처럼 유지 보수가 어렵다.

  • 일반적인 MVC보다 명확한 구분으로 인하여 코드가 길다.