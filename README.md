# Ruby & Jekyll 환경 설정 메모

## 1. Gem 권한 에러 해결
시스템 루비 사용 시 발생하는 권한 에러(`write permissions`) 해결을 위해 `rbenv` 사용을 권장합니다. (sudo 사용 금지)

### rbenv 설치 및 루비 설정
```bash
brew update
brew install rbenv ruby-build

# 설치 가능 버전 확인 및 설치
rbenv install -l
rbenv install 4.0.0
rbenv global 4.0.0
```

## 2. 셸 환경 설정 (.zshrc)
```bash
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
source ~/.zshrc

# 설정 확인 (경로에 .rbenv/shims 포함 확인)
which ruby
```

## 3. Ruby 상위 버전 라이브러리 에러
Ruby 3.4+ 버전에서 `ostruct` 등 기본 라이브러리가 제외되어 발생하는 에러 해결 방법입니다.

### 해결 방법
`Gemfile`에 누락된 라이브러리를 명시적으로 추가 후 설치합니다.
```ruby
gem "ostruct"
```
```bash
bundle install
```

## 4. 로컬 실행 방법
```bash
bundle exec jekyll serve
```
