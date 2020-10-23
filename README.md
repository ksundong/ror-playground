# ROR Playground

루비 온 레일즈 공식문서를 보면서 레일즈를 써본다.

## 레일즈의 철학

- 반복하지 말라(Don't Repeat Yourself): DRY 원칙을 따른다 같은 정보를 반복해서 작성하지 않으므로써 우리의 코드는 더 유지보수하기 좋고, 더 확장가능성이 있으며, 덜 버그를 생성하게 된다.
- 설정 위의 규칙(Convention over Configuration): Rails는 웹 애플리케이션에서의 많은 일들에 대한 가장 좋은 방법에 대한 의견이 있으며, 끝없는 설정파일들로 세부적인 것들을 설정하도록 하지 않고, 규칙들의 집합을 기본적으로 사용한다. (아마 스프링부트가 영향을 받은 대목인 것 같다.)  
   비교 대상은 Django로 Python의 철학인 Zen of Python의 explicit is better than implicit.을 따른다.

## 가이드를 따라가는데 필요한 개발 환경

- Ruby(2.5.0+)
- SQLite3
- Node.js(8.16.0+)
- Yarn

## Rails 설치하기 / Mac에서 오류 발생시 대처법

`gem install rails` 로 설치합니다.

### Mac에서 발생한 문제

이상하게 자꾸 rails가 설치되지 않았습니다. 인터넷의 솔루션들을 봐도 동작하지 않는 문제가 있었습니다. (내 시간...)

그 이유는 Mac에 default로 설치된 ruby를 이용해 설치하려고 했기 때문이었습니다.

`brew link --overwrite ruby` 를 입력했고, 혹시 몰라서 아래의 것들도 입력해주었습니다. 저는 zsh을 사용중입니다.

일단 link overwrite만 적용해보시고 안된다면 아래의 것을 입력해보시기 바랍니다.

```shell
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc
export LDFLAGS="-L/usr/local/opt/ruby/lib"
export CPPFLAGS="-I/usr/local/opt/ruby/include"
export PKG_CONFIG_PATH="/usr/local/opt/ruby/lib/pkgconfig"
echo 'export PATH="'$(gem env gemdir)'/bin:$PATH"' >> ~/.zshrc
```

## Rails blog 애플리케이션 만들기

`rails new blog` 로 생성할 수 있습니다.

`cd blog` 로 생성된 애플리케이션을 확인할 수 있습니다.

default로 git이 자동으로 초기화 되어있습니다. 

## Rails 서버 구동하기

`rails server` 명령어로 실행합니다.

개발 모드에서는 서버를 재시작할 필요없이 변경된 파일이 자동으로 선택됩니다.

## Rails로 간단한 Controller와 View 구성하기

`rails generate controller Welcome index`

Controller의 목적은 특정한 요청을 받아주는 것이고, Router가 요청을 어떤 컨트롤러에게 전달할지 결정합니다.

같은 요청이지만 다른 actions로 요청이 처리될 수 있고, 각 actions의 목적은 정보를 수집(?)해서 view에 제공해주는 것입니다.

View의 목적은 사람이 읽을 수 있는 형식으로 정보를 보여주는 것입니다. View에서 정보가 수집되는 것이 아니라, Controller에서 수집된다는 것이 중요한 점입니다. 뷰에는 단순히 해당 정보만 표시되어야 합니다. 기본적으로, 뷰 템플릿은 eRuby라는 언어로 작성되고, eRuby는 사용자에게 전송되기 전에 Rails의 요청 사이클에 의해 처리됩니다.(`.erb`)

## Rails 루트 URL 지정하기

텍스트 에디터로 `config/routes.rb` 파일을 수정합니다.

`root 'welcome#index'` 를 마지막 줄(`end` 윗부분)에 추가해줍니다.

## References

[Ruby On Rails Refernce Document](https://guides.rubyonrails.org/getting_started.html)

[Convention over Configuration wikipedia](https://en.wikipedia.org/wiki/Convention_over_configuration)

