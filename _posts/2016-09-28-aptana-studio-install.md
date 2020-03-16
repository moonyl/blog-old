---
title: "Aptana studio 3 설치"

categories:
  - web-devel
tags:
  - aptana
  - git
  - nanum
  - nodejs
last_modified_at: 2016-09-28T04:40:00-05:00
---
HTML5, CSS3 개발 환경 중에 SK T 아카데미에서 추천하는 것은 aptana studio 3 라는 툴이다. eclipse 에 plugin 형태로 사용할 수도 있고, standalone 형태로도 사용이 가능하다고 한다. 이전에 여러 IDE 중에서 Webstorm 6 가 가장 우수해 보인다고 글을 남긴 적이 있었는데, 유료 툴이다보니, 아무래도 aptana studio 3 를 추천하는 것으로 보인다.

aptana studio 3 는 다음 사이트에서 다운로드 받을 수 있다.

[Aptana Studio 3](http://www.aptana.com/)

하지만 받은 파일만으로는 설치가 되지 않고 이전에 한 가지 더 설치해줘야 한다.

[nodejs](https://nodejs.org/en/)

뒤에 받은 nodejs installer 를 설치한 후, 앞에서 받은 aptana studio 3을 설치를 하도록 한다.

install 이 되지 않는 상황을 한 가지 더 발견했다.

installer_git_windows.exe 를 설치 못한다는 내용인 듯 한데, 이럴 때는 직접 다운로드 받아서 설치해주면 된다.

[git for windows](https://gitforwindows.org/)

download 버튼을 눌러 받아서 설치한다.

HTML5/CSS3 관련 작업을 하기 위해서는 project 를 webproject 로 선택해주면 된다.

생성된 web project 에 마우스 오른쪽 버튼 클릭을 해서 new from template 으로 파일을 만들면 되는데, 이상하게도 내가 설치한 프로그램에서는 HTML5 template 이 나타나지 않는다. 방법을 좀 찾아봐야 할 것 같다.

코딩을 하는데 도움이 되는 폰트를 하나 추천 받았다. 나눔 코딩 폰트라는 것인데, 폰트가 고딕체로 글자가 명확하게 보여서 편한감이 있다. 글자간의 구별이 잘 되어서 오타의 염려도 좀 줄어들 것 같다.

[나눔글꼴](https://hangeul.naver.com/2017/nanum)

windows 메뉴에 preferences 항목을 선택해서 font 를 검색하면 appearance 설정 하위에 colors and fonts 가 보인다. 

![](https://t1.daumcdn.net/cfile/tistory/224FCF4857EB529209)

나눔고딕코딩 폰트를 적용하니 글자가 명확해 보인다. 다른 에디터에도 이 폰트를 적용해야 겠다.

실행할 때 구동되어야 할 웹브라우저를 지정할 수 있다. Run 메뉴에서 Run configuration 을 실행해서 Web browser 항목을 설정해주면 된다. chrome 이 설치되어 있는 위치를 찾는 것이 쉽지 않았다. 내 컴퓨터에는 C:\Program Files (x86)\Google\Chrome\Application 에 설치되어 있었다.
![](https://t1.daumcdn.net/cfile/tistory/241C574C57EB551C09)