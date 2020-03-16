---
title: "강력한 QA 를 갖췄지만 드러난 삼성의 한계"

categories:
  - essay
tags:
  - 삼성
  - QA
last_modified_at: 2016-10-25T04:40:00-05:00
---

갤럭시 노트7 의 생산 중단이라는 결과로 약간의 충격을 받았다.

하드웨어 품질 하나만큼은 다른 어떤 회사보다 탄탄하다는 자부심이 있는 회사로 여겼기 때문이다.

삼성전자에서 3년 정도 몸담고 있었지만, QA 조직은 정말 강력하다. 어떻게 보면 개발은 QA 의 눈치를 봐야하는 상황도 많았다. 품질을 우선시 하는 회사로의 면모를 유감없이 보여주는 부분이다.

그러나, 이번에 발생한 문제는 이러한 삼성에서도 나타날 수 있다는 걸 보여준다. 무엇이 문제였을까. 나는 나름 ["샘플테스트 한계와 갤럭시 노트7"](https://news.mt.co.kr/mtview.php?no=2016102417091252123&outlink=1&ref=https%3A%2F%2Fcraftsmanship.tistory.com) 이라는 칼럼의 내용에 대체로 공감한다.

QA 의 테스트 내용이 넓은 범위의 상황을 커버하고 꼼꼼하게 확인하는 것도 중요하지만, 무엇보다 실제 환경에서 나타날 수 있는 부분인가 라는 부분에 더 촛점이 맞춰져야 할 것 같다. 국제기준의 안정성을 통과하고 엄청난 사양을 갖추는 것은 사용자에게 좋은 첫인상을 줄지는 모른다. 하지만 안정성 테스트 부분의 현실성이 떨어지고 사양이 궁극적인 목적을 벗어난 것이라면, 개발도 더디게 하고, 실제 사용자가 사용하고 난 이후, 그 제품에 대해 느끼는 바가 다른 제품보다 뛰어나다고 느끼지 못할 수 있을 것이다.

분명, 갤럭시 노트7은 삼성 자체 내에서 테스트했을 때는 발화의 문제가 없었을 것이다. 엄청난 사용자 중에서 아주 몇몇에게만 발생하는 - 내가 개발했을 때는 주로 단품 문제로 돌리고 싶어했던 상황이다 - 문제일 수도 있다. 그러나, 발화의 문제라는 것은 사용자와 그 주변에 있는 사람의 안전에도 위험을 가할 수 있기 때문에 발생하지 않아야 하는 것이다. 

초기 문제 발생 시에 대응하는 방법도 아쉬운 부분이 있었다. 원인을 정확히 밝히지 않은 상황에서 밀어붙이기 방식으로 리콜을 진행한 것이 아닌가 한다. 결국 단종이라는 조금 극단적인 결과로 나타난 것 같기는 하지만, 명확하게 원인을 밝혀서 다음에 노트8에는 해결이 완벽하게 되어서 출시되길 바란다.

QA 테스트가 좀 더 현실성에 가까운 방향으로 변화가 되고, 제품의 사양도 목적에 부합하게, 그리고 개발에서는 원인에 기반한 해결이 이뤄져야 다시 이러한 상황이 재발하지 않도록 할 수 있을 것이다.

끝으로, 칼럼 내용 중에 표현이 멋있다고 느낀 부분을 기록해 둔다.
> 이는 휴대폰 품질테스트를 할 때 기능들이 정상 동작하는지, 배터리 등 부분의 하자는 없는지 등 '전문의'적인 요소에 중점을 두고 검사를 하는 프레임의 한계가 있을 수 있음을 시사한다. 소비자의 실제 사용환경을 중점에 두고 전체적인 구동상황을 보고 에러를 잡아내는 '가정의학적'인 테스트가 더 중요하다는 생각이다.