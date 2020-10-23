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

## References

[Ruby On Rails Refernce Document](https://guides.rubyonrails.org/getting_started.html)
[Convention over Configuration wikipedia](https://en.wikipedia.org/wiki/Convention_over_configuration)

