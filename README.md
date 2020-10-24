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

## Rails로 간단한 REST 기능 구현하기

`config/routes.rb` 파일에 다음과 같이 작성합니다.

```ruby
Rails.application.routes.draw do
  get 'welcome/index'
 
  resources :articles
 
  root 'welcome#index'
end

```

`resources` 는 표준 REST resource를 정의하는 메서드입니다.

`rails routes` 를 입력해봅시다. 입력하면 이미 정의된 모든 표준 RESTful action들에 대한 route들을 볼 수 있습니다.

우리가 입력한건 `articles` 인데, 단수형태인 `article` 이라는 단어가 의미있게 사용됨을 알 수 있습니다.

## Rails로 글 작성과 조회 기능 구현하기

### 기초작업

먼저, 우리는 글 작성 화면을 만들고자합니다. 주소는 `/articles/new` 가 좋겠네요.

하지만 해당 주소로 접속하면, 라우팅 에러 페이지가 출력됩니다. 루트는 요청에 대해 서빙하기 위해서 컨트롤러가 필요합니다. 이 문제를 해결하는 방법은 간단합니다. `ArticlesController` 를 생성해주면 됩니다.

```shell
$ rails generate controller Articles
```

그리고 텍스트 편집기로 `app/controllers/articles_controller.rb` 를 열면 비어있는 컨트롤러를 확인하실 수 있습니다.

이 컨트롤러는 `ApplicationController` 를 상속합니다. 이 컨트롤러에 메서드를 정의해 액션을 만들어줄 수 있습니다.

참고로 컨트롤러는 `public` 이어야 합니다.

---

이제 다시 접속해보면 action을 찾을 수 없다는 오류가 나옵니다.

직접 컨트롤러에 액션을 정의해주려면 `new` 라는 메서드를 컨트롤러에 정의해주면 됩니다.

```ruby
class ArticlesController < ApplicationController
  def new
  end
end
```

이제 새로고침 해보면, 다른 에러가 나옵니다. 바로 요청에 대한 view를 가질 것으로 Rails가 동작하는데 해당하는 view가 없기 때문에 생기는 오류입니다.

에러 메시지를 읽어보면 `articles/new` 템플릿이 없기 때문에 발생함을 알수 있습니다. API로 동작하는 경우에 `204(No Content)` 응답인 경우엔 템플릿을 필요로 하지 않음을 알 수 있습니다.

레일즈는 `articles/new` 를 먼저 탐색합니다. 그 다음 `application/new` 를 탐색합니다. 왜일까요? `ArticlesController` 가  `ApplicationController` 를 상속하기 때문입니다.

우리가 브라우저로 요청했기 때문에, 요청의 포맷은 `text/html` 로 결정되었습니다. 따라서 Rails는 HTML 템플릿을 찾게됩니다.

이 경우에 응답해 줄 수 있는 가장 간단한 템플릿의 위치는 `app/views/articles/new.html.erb` 입니다. 파일의 확장자는 매우 중요합니다. 첫번째 확장자는 이 템플릿의 포맷을 의미하고, 두번째 확장자는 이 템플릿을 렌더하는데 사용될 핸들러를 결정합니다.

Rails는 `articles/new` 라는 템플릿을 `app/views` 에서 찾게됩니다. 이 포맷은 `html` 형식만 될 수 있으며, HTML 의 기본 핸들러는 `erb` 입니다. Rails는 다른 핸들러를 다른 포맷에 사용할 수 있습니다. `builder` 핸들러는 XML 템플릿을 빌드하는데 사용되고, `coffee` 핸들러는 CoffeeScript를 사용해 자바스크립트 템플릿을 빌드하는데 사용됩니다. `ERB` 는 HTML에 Ruby를 내장해서 사용할 수 있게끔 디자인 된 언어입니다. 이를 사용해 새로운 HTML 폼을 만들 수 있습니다.

따라서 우리가 만들 파일은 `articles/new.html.erb` 가 됩니다. 그리고 이 파일은 `app/views` 디렉토리 안에 있어야 합니다.

새로운 파일을 `app/views/articles/new.html.erb` 로 작성하세요. 그리고 다음 내용을 작성하세요.

```erb
<h1>New Article</h1>
```

이제 새로고침 해봅시다. 이제 페이지가 정상적으로 나오는 것을 확인할 수 있습니다.

루트, 컨트롤러, 액션, 뷰가 이제 조화롭게 동작합니다! 이제 새로운 기사를 작성하기 위한 폼을 만들어야 할 때입니다.

### 첫번째 폼

이 템플릿으로 폼을 만들기 위해선, 폼 빌더를 사용해야 합니다. Rails에서 폼 빌더를 사용하는 첫번째 방법은 `form_with` 라는 헬퍼 메서드를 사용하는 것입니다. 이 메서드를 사용하기 위해서 다음의 코드를 `app/views/articles/new.html.erb` 에 추가하세요.

```erb
<%= form_with scope: :article, local: true do |form| %>
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
<% end %>
```

새로고침 하면 됩니다. 참 쉽죠?

`form_with` 메서드를 호출하면, 이 폼을 특정하기 위한 스코프를 입력해줍니다. 여기선 `:article` 이라는 스코프를 주었어요. 이렇게 지정하면 `form_with` 헬퍼에 이 폼이 어떤 용도로 사용되는지 말해줄 수 있습니다.

이 메서드의 블럭 안에는 `FormBuilder` 오브젝트가 `form` 으로 표현되어 있습니다. 이 오브젝트는 두 개의 라벨과, 두개의 텍스트 필드 그리고 `submit` 버튼으로 이루어져 있습니다.

이 폼에는 하나의 문제점이 있습니다. 생성된 HTML 페이지의 소스를 확인해보면, `action` 속성이 `/articles/new` 를 가리키고 있음을 볼 수 있습니다. 이 경로는 바로 지금 있는 페이지로 이동하고, 해당 경로는 새 기사의 폼을 표시하는데만 사용되어야 하기 때문에 문제가 됩니다.

이 폼은 다른 곳으로 가기 위해서 다른 URL을 필요로 합니다. 이것은 간단하게 `form_with` 의 `:url` 옵션을 지정하는 것으로 됩니다. 일반적으로 Rails에서 새로운 폼을 제출하는 액션을 "create" 라고 하며, 폼은 그 action을 가리켜야 합니다.

`form_with` 라인을 다음과 같이 수정하세요.

```erb
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
```

이 예제에서, `articles_path` 헬퍼는 `:url` 옵션으로 전달됩니다. Rails가 이것으로 무엇을 하는지 보기 위해서, `rails routes` 의 출력을 다시 봅시다.

`articles_path` 헬퍼는 Rails에게 articles 접두사와 관련된 URI 패턴을 가리키도록 알려주고, 폼은 기본적으로 해당 경로로 `POST` 요청을 전송하도록 합니다. 이는 현재 컨트롤러인 `ArticlesController` 의 `create` 액션과 관련이 있습니다.

폼과 이와 연관된 경로가 정의되었다면, 폼을 채우고, submit 버튼을 눌러 새로운 기사 생성 프로세스를 시작할 수 있습니다. 한 번 해보세요. 폼을 제출하면 익숙한 에러를 볼 수 있을겁니다.

이제 `create` 액션을 `ArticlesController` 에 생성하는 것이 동작하도록 만드는 방법이겠죠?

> 원래 `form_with` 는 Ajax를 사용해서 전체페이지 리디렉션을 생략합니다. 이 가이드를 쉽게 만들기 위해 `local: true` 옵션으로 비활성화 했습니다.

## References

[Ruby On Rails Refernce Document](https://guides.rubyonrails.org/getting_started.html)

[Convention over Configuration wikipedia](https://en.wikipedia.org/wiki/Convention_over_configuration)

