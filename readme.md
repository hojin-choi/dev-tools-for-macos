# dev-environment-macos

macos 환경에서 개인 개발환경 설정을 위한 가이드문서이다.

## iterm2 설치
[iterm2](https://iterm2.com/) 사이트에서 app을 다운로드 받는다.

## homebrew 설치
macos 터미널에서 jq, python, mysql 등 다양한 application의 package manager 역할을 한다.
[homebrew](https://brew.sh/)
curl을 통해 설치할 수 있다.
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## oh-my-zsh 설치
bash 보다 zsh 을 사용하면 여러가지로 편리하다.
[oh-my-zsh](https://ohmyz.sh/)
curl을 통해 설치할 수 있다.
```sh
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## iterm / oh-my-zsh theme 변경
zsh theme은 agnoster, color는 espresso, font는 `Inconsolata for Powerline` 을 설정하고 사용 중이다.

주요설정내용은 [해리의유목코딩-Oh My ZSH+ iTerm2로 터미널을 더 강력하게](https://medium.com/harrythegreat/oh-my-zsh-iterm2%EB%A1%9C-%ED%84%B0%EB%AF%B8%EB%84%90%EC%9D%84-%EB%8D%94-%EA%B0%95%EB%A0%A5%ED%95%98%EA%B2%8C-a105f2c01bec) 글을 주로 참고했다.

### zsh theme 변경
1. `~/.zshrc` 파일 내 `ZSH_THEME` 값을 변경한다
`ZSH_THEME="agnoster"`

### iterm color 변경
1. [iterm2-color-schema](https://iterm2colorschemes.com/)를 통해 color preset을 다운로드 받는다.
2. iterm2 > preferences > profiles > color presets > imports... 클릭 > 다운로드받은 폴더 내 `*.itermcolors` 를 import 한다.

### iterm font 변경
1. [powerline fonts](https://github.com/powerline/fonts)를 설치한다.
2. iterm2 > preferences > profiles > text > `Inconsolata for Powerline` 14px 을 설정한다.

### zsh agnoster 테마 customizing
hostname을 제외한 user 정보 출력, 새라인에서 명령어 입력 등을 적용하기위해 agnoster theme 파일을 수정한다.
[~/.oh-my-zsh/themes/agnoster.zsh-theme](customized/agnoster.zsh-theme) 내 아래 내용을 반영한다.

```sh
# hostname을 제외한 user 만 출력한다
prompt_context() {
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    # 아래 원본 라인 주석처리
    # prompt_segment black default "%(!.%{%F{yellow}%}.)%n@%m"
    # hostname을 제외한 user 만 출력한다
    prompt_segment black default "%(!.%{%F{yellow}%}.)%n"
  fi
}

# 새 라인에서 명령어 입력이 가능하게 function을 추가한다
prompt_newline() {
  if [[ -n $CURRENT_BG ]]; then
    echo -n "%{%k%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR
%(?.%F{$CURRENT_BG}.%F{red})❯%f"
  else
    echo -n "%{%k%}"
  fi
  echo -n "%{%f%}"
  CURRENT_BG=''
}

# build_prompt 내 prompt_end 앞에 prompt_newline을 추가한다
build_prompt() {
  # 생략
  prompt_newline
  prompt_end
}

```

### zsh syntax highlight 적용
```sh
# brew를 통해 설치해줍니다.
brew install zsh-syntax-highlighting

# 플러그인을 적용합니다.
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

## vscode 설치
[vscode](https://code.visualstudio.com/) 를 설치한다.

## vscode 주요 plugin 설치

### EditorConfig for VS Code
EditorConfig를 인식해서 자동으로 탭 스타일 등을 적용해주는 플러그인

### TSLint
typescript 스타일 가이드 위반을 검사하기 위한 플러그인 
> TSLint는 deprecated되고 ESLint로 통합된다.

### ESLint
javascript, typescript 스타일 가이드 위반을 검사하기 위한 플러그인

### GitLens - Git supercharged
vscode editor 내에서 해당 라인을 수정한 commit / PR 정보 등을 볼 수 있다.

### Git History
file 단위 git history를 볼 때 유용하다.

### Bracket Pair Colorizer2
vscode editor 내 코드 bracket을 깊이 별로 색을 구분해준다.

### TODO Highlight
vscode editor 내 코드의 TODO, FIXME 등을 highlight 해준다.

### Todo Tree
TODO / FIXME 등을 tree 탐색기로 모아서 볼 수 있다.

### GraphQL
graphql 코드 syntax highlighting 등을 해준다.

### vscode-styled-components
styled component 코드 Syntax highlighting and IntelliSense 등을 해준다.

### CloudFormation
CFN 코드작성에 도움을 준다.

### YAML
YAML 문법오류를 알려준다 코드작성에 도움을 준다.

### Material Icon Theme
vscode workspace 내 파일 종류/확장자 별 아이콘을 제공한다.

### One Monokai Theme
vscode color theme

## vscode setting.json
vscode 내 [setting.json](customized/vscode-setting.json) 내 아래 내용을 반영한다.
- vscode terminal 내 espresso color / Inconsolata for Powerline font 등을 적용한다.
- CFN yaml custom tag 를 정의한다.