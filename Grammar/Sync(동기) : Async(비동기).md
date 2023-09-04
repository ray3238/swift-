# Sync(동기) / Async(비동기)

------

메인 스레드 (thread 1)가 모든 일을 하고 있는 상황

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d0ea0830-5a73-4160-abfb-bde53bb3e165/Untitled.png)

쏟아지는 일들을 **다른 스레드들로 나누기**로 결심한 메인 스레드..!! 이를 위해서 **작업들을 queue로 보내기만 하면** 된다! 라고 했어요~

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/90109182-fc40-43da-bf63-9f3db7ea58a2/Untitled.png)

**스레드에 분배는 GCD가 알아서** 해준다는 말에 일단 큐에 작업 한 개를 보낸 우리의 메인 스레드..!

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da18186a-cb0b-408c-968c-0abbc9b38f0c/Untitled.png)

queue에 작업을 보내고 난 후 메인 스레드의 행동은?

1. **신경 끄고** 자기한테 쌓여있는 **다음 일**을 한다(sync)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3f41b48-88c7-4456-93a9-ceb9c0e026b7/Untitled.png)

2. **끝날 때까지 기다린 후**에 자기한테 쌓여있는 **다음 일**을 한다(async)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bc56310f-11d8-4ae7-beb3-555d8c8dbcdd/Untitled.png)

- 쌓여있는 **다음 일** : “**다음 라인을 실행한다”**

## **비동기**

**queue에 보낸 작업에 대해서는 더 이상 신경 안 씀**

바로 다른 일을 해버림. **보낸 작업에 걸렸을 시간 만큼 또 다른일을 할 수 있음**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63cfb6e7-6d9c-4828-b050-713f11cf81bb/Untitled.png)

- 코드 예;

  ```swift
  DispatchQueue.global().async {
    //task
  }
  ```

  - DispatchQueue: iOS에서 동시성 프로그래밍을 돕기 위해 제공하는 queue
  - global: DispatchQueue의 종류
  - async: 비동기

  코드 해석 ::

  **원래의 작업이 진행되고 있던 메인 스레드에서 global dispatch queue로 task를 보낸 후,**

  **해당 작업이 끝나기를 기다리지 않고 이어서 할 일을 한다.**

[더 긴 코드 예;](https://www.notion.so/be8e43f6f7814299a0e86401bfc816e8?pvs=21)

**“ 일을 queue에 보내기만 하고 신경을 안쓰고 있는데 .. 그럼 실제 해당 작업이 언제 끝나는지는 어떻게 알지? ”**

**클로저**를 통해 **해당 시점**을 알려줌

이를 `completionHandler` 또는 `completion` 라고 부름

## **동기**

**queue에 보낸 작업이 완료 될 때까지 기다린 후 다음 줄로 넘어갑니다**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99027e8d-e2c8-49d3-9c41-d02907b9ae19/Untitled.png)

- 코드 예;

  ```swift
  DispatchQueue.global().sync {
    //task
  }
  ```

  코드 해석 ::

  원래의 작업이 진행되고 있던 메인 스레드에서 global dispatch queue로 task를 보낸 후,

  다음 라인 실행을 위해 해당 작업이 끝나기를 기다린다

[더 긴 코드 예;](https://www.notion.so/9818a8effa8e44da991bf867a5650925?pvs=21)

------

## 그래서 비동기 처리는 왜 필요한가?

- 당연히 **시간 절약**

그리고 시간이 많이 드는 작업의 대부분은 **서버와의 통신**이기 때문에

**네트워크와 관련된 작업들은 내부적으로 비동기적으로 구현**이 되어있음.

------

API 연동을 도와주는 Alamofire, URLSession, Moya는 내부적으로 비동기를 포함하고 있다.