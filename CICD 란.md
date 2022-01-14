# CI/CD 란?

> TL;DR 
>
> 어플리케이션 개발 단계 부터 배포 때까지의 모든 단계를 자동화를 통해서 조금 더 효율적이고 빠르게 사용자에게 빈번히 배포할 수 있도록 만드는 것을 말한다. 

## CI 란?

* `Continuous Integration(지속적인 통합)` 을 이야기 한다. 

* 버그수정, 새로 만드는 기능들이 main Repository 에 주기적으로 빌드되고 테스트 되어서 merge 되는 것을 말한다. 

* CI 는 2가지를 포인트로 잡고있다.

  1. 코드 변경사항을 **주기적**으로 **빈번하게** 머지해야 한다. 

  2. 통합을 위한 단계(빌드, 테스트, 머지)의 자동화

     * 주기적으로 Merge 된 코드의 변경 사항이 자동으로 build 가 되어서 코드 변경사항 이후에도 build 가 성공적으로 되는지  확인이 되어야 한다.
     * 새롭게 추가된 코드 뿐만 아니라 변경된 코드가 기존의 시스템에 버그를 만들지 않았는지 자동으로 테스트 까지 되어야 한다. 

* CI 순서는 아래와 같다.
  1. 개발자가 새롭게 작성한 코드를 자신이 생성한 브랜치에 커밋한다.
  2. 협업하고 있는 동료 개발자가 있다면 코드를 열심히 리뷰 받는다. 
  3. 만약 LGTM(Look good to me) 를 받았다면 main 브랜치 또는 적절한 브랜치에 Merge 하고 그렇지 않으면 다시 수정후 1번으로 돌아간다.
  4. Merge를 하면 자동으로 팀에서 만든 CI script를 통해서 추가된 코드와 함께 이 Repository 가 빌드가 되고 빌드가 잘 된다면 팀에서 작성한 unit test, integration test, 등등 여러가지 테스트들도 스크립트를 통해서 실행된다. 
  5. 만약 빌드와 모든 테스트가 모두 통과하면 그린 라이트가 둘중에 하나라도 실패하면 레드 라이트가 나오고 해당 커밋을 한 개발자에게 메일로 알려줍니다. 

## CI의 장점

* 주기적으로 머지를 하기 때문에 개발 생산성이 향상 된다. 
* 문제점을 빠르게 발견될 수 있습니다.
  * 버그가 수정하기 용이합니다.



## CD 란?

* `Continuous Deployment(지속적인 배포) 또는 Continuous Delivery(지속적인 제공)`를 이야기 합니다.
  * `Continuous Deployment` 와 `Continuous Delivery` 는 약간 다릅니다. 
  * `Continuous Delivery` 는 최종 배포를 수동으로 진행하는 것이고 `Continous Deployment` 는 최종 배포를 자동으로 진행하는 것입니다. 
* <img width="1744" alt="Screen Shot 2022-01-13 at 1 22 53 PM" src="https://user-images.githubusercontent.com/55227984/149269614-f94629aa-bb4e-40a3-831c-f211d35540f6.png">

## 최종 순서

<img width="1753" alt="Screen Shot 2022-01-13 at 1 27 15 PM" src="https://user-images.githubusercontent.com/55227984/149269680-d4b4ff3d-8a00-478d-9749-1f7c5710f187.png">

## 사용될 수 있는 도구들

* Jenkins
* Buildkite
* Github Actions
* Travis CI
* circleci
* Bitbucket Pipelines
* GitLab CI/CD