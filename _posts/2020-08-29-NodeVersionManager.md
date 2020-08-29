---
title: 리액트 개발 환경 설치하기 - NVM 설치
updated: 2020-08-29 22:40
category: javascript
---
**참고문서** : 실리콘밸리 개발 방법으로 배우는 Do it 리액트 프로그래밍 정석

# 리액트 개발 환경 설치하기
## 노드 제이에스란
- 구글에서 공개한 소프트웨어
- V8 엔진을 기반으로 만든 자바스크립트 런타임 도구
- 웹 브라우저가 아닌 컴퓨터(or 서버)에서 JS를 실행할 수 있게 해줌
   
노드 패키지 매니저(npm)와 노드 제이에스의 버전 관리를 위해 nvm 설치를 해야 함.   
책에서 설명된 자료 대신 Mac/linux 용 nvm 을 설치해야 함
   
[Mac/linux 용 NVM 설치 가이드](https://github.com/nvm-sh/nvm)

## NVM 셋팅
### 1. NVM 설치하기
```terminal
# 둘 중 하나 입력
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
# OR
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

```terminal
zeongyun@zeongyunui-MacBook-Pro nvm % curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 13527  100 13527    0     0  34159      0 --:--:-- --:--:-- --:--:-- 34159
=> Downloading nvm from git to '/Users/zeongyun/.nvm'
=> Cloning into '/Users/zeongyun/.nvm'...
remote: Enumerating objects: 290, done.
remote: Counting objects: 100% (290/290), done.
remote: Compressing objects: 100% (257/257), done.
remote: Total 290 (delta 35), reused 97 (delta 20), pack-reused 0
Receiving objects: 100% (290/290), 163.27 KiB | 350.00 KiB/s, done.
Resolving deltas: 100% (35/35), done.
=> Compressing and cleaning up git repository

=> Profile not found. Tried ~/.bashrc, ~/.bash_profile, ~/.zshrc, and ~/.profile.
=> Create one of them and run this script again
   OR
=> Append the following lines to the correct file yourself:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm

=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
```
설치는 됐으나 Bash Profile이 존재하지 않아 알림 메시지가 밑에 표시된 걸 확인할 수 있음.   
.bash 에 해당 내용을 수동으로 안내에 따라 환경 변수 및 명령어 설정을 해줘야 했음.

```terminal
source ~/.bash
```

nvm 버전 확인 커맨드 `nvm ls`를 입력
```terminal
nvm ls
            N/A
iojs -> N/A (default)
node -> stable (-> N/A) (default)
unstable -> N/A (default)
nvm_list_aliases:36: no matches found: /Users/zeongyun/.nvm/alias/lts/*
```
설치가 안된 항목이 더러 있다는 것을 확인

### 2. NVM 버전 확인하기
```terminal
nvm -v
Node Version Manager (v0.35.3)

Note: <version> refers to any version-like string nvm understands. This includes:
  - full or partial version numbers, starting with an optional "v" (0.10, v0.1.2, v1)
  - default (built-in) aliases: node, stable, unstable, iojs, system
  - custom aliases you define with `nvm alias foo`

 Any options that produce colorized output should respect the `--no-colors` option.

Usage:
  nvm --help                                Show this message
  nvm --version                             Print out the installed version of nvm
  nvm install [-s] <version>                Download and install a <version>, [-s] from source. Uses .nvmrc if available
    --reinstall-packages-from=<version>     When installing, reinstall packages installed in <node|iojs|node version number>
    --lts                                   When installing, only select from LTS (long-term support) versions
    --lts=<LTS name>                        When installing, only select from versions for a specific LTS line
    --skip-default-packages                 When installing, skip the default-packages file if it exists
    --latest-npm                            After installing, attempt to upgrade to the latest working npm on the given node version
    --no-progress                           Disable the progress bar on any downloads
  nvm uninstall <version>                   Uninstall a version
  nvm uninstall --lts                       Uninstall using automatic LTS (long-term support) alias `lts/*`, if available.
  nvm uninstall --lts=<LTS name>            Uninstall using automatic alias for provided LTS line, if available.
  nvm use [--silent] <version>              Modify PATH to use <version>. Uses .nvmrc if available
    --lts                                   Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                        Uses automatic alias for provided LTS line, if available.
  nvm exec [--silent] <version> [<command>] Run <command> on <version>. Uses .nvmrc if available
    --lts                                   Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                        Uses automatic alias for provided LTS line, if available.
  nvm run [--silent] <version> [<args>]     Run `node` on <version> with <args> as arguments. Uses .nvmrc if available
    --lts                                   Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                        Uses automatic alias for provided LTS line, if available.
  nvm current                               Display currently activated version of Node
  nvm ls [<version>]                        List installed versions, matching a given <version> if provided
    --no-colors                             Suppress colored output
    --no-alias                              Suppress `nvm alias` output
  nvm ls-remote [<version>]                 List remote versions available for install, matching a given <version> if provided
    --lts                                   When listing, only show LTS (long-term support) versions
    --lts=<LTS name>                        When listing, only show versions for a specific LTS line
    --no-colors                             Suppress colored output
  nvm version <version>                     Resolve the given description to a single local version
  nvm version-remote <version>              Resolve the given description to a single remote version
    --lts                                   When listing, only select from LTS (long-term support) versions
    --lts=<LTS name>                        When listing, only select from versions for a specific LTS line
  nvm deactivate                            Undo effects of `nvm` on current shell
  nvm alias [<pattern>]                     Show all aliases beginning with <pattern>
    --no-colors                             Suppress colored output
  nvm alias <name> <version>                Set an alias named <name> pointing to <version>
  nvm unalias <name>                        Deletes the alias named <name>
  nvm install-latest-npm                    Attempt to upgrade to the latest working `npm` on the current node version
  nvm reinstall-packages <version>          Reinstall global `npm` packages contained in <version> to current version
  nvm unload                                Unload `nvm` from shell
  nvm which [current | <version>]           Display path to installed node version. Uses .nvmrc if available
  nvm cache dir                             Display path to the cache directory for nvm
  nvm cache clear                           Empty cache directory for nvm

Example:
  nvm install 8.0.0                     Install a specific version number
  nvm use 8.0                           Use the latest available 8.0.x release
  nvm run 6.10.3 app.js                 Run app.js using node 6.10.3
  nvm exec 4.8.3 node app.js            Run `node app.js` with the PATH pointing to node 4.8.3
  nvm alias default 8.1.0               Set default node version on a shell
  nvm alias default node                Always default to the latest available node version on a shell

Note:
  to remove, delete, or uninstall nvm - just remove the `$NVM_DIR` folder (usually `~/.nvm`)
```
npm(Node Package Manager)설치가 진행되지 않아 그런지 이런 결과가 출력된다.

### 3. NVM으로 노드제이에스 설치하기
최신버전은 12버전이지만 현재 현업에서 가장 많이 사용하는 노드제이에스 버전이 8이므로 8버전으로 설치
```terminal
nvm install 8.10.0
Downloading and installing node v8.10.0...
Downloading https://nodejs.org/dist/v8.10.0/node-v8.10.0-darwin-x64.tar.gz...
########################################################################################## 100.0%
Computing checksum with shasum -a 256
Checksums matched!
Now using node v8.10.0 (npm v5.6.0)
Creating default alias: default -> 8.10.0 (-> v8.10.0)
```

### 4. NVM이 설치한 노드제이에스 사용 설정하기
NVM으로 설치한 노드제이에스 8.10.0 버전을 사용할 수 있도록 명령어를 입력. 그다음 노드제이에스와 npm의 버전을 확인
```terminal
nvm use 8.10.0
Now using node v8.10.0 (npm v5.6.0)

node -v
v8.10.0

npm -v
5.6.0
```
   
## yarn과 create-react-app 설치하기
### 1. yarn 설치하기
`create-react-app`은 리액트 프로젝트에 필요한 패키지들을 묶어 리액트 앱을 생성해주는 도구이다. 
만약 create-react-app이 없다면 리액트 프로젝트에 필요한 패키지를 일일이 찾아 `package.json`에 추가해야 한다. 
create-react-app은 `npm`을 통해 설치해야 하는데 `yarn`을 통해 설치할 수도 있다.
   
```terminal
npm install -g yarn
/Users/zeongyun/.nvm/versions/node/v8.10.0/bin/yarn -> /Users/zeongyun/.nvm/versions/node/v8.10.0/lib/node_modules/yarn/bin/yarn.js
/Users/zeongyun/.nvm/versions/node/v8.10.0/bin/yarnpkg -> /Users/zeongyun/.nvm/versions/node/v8.10.0/lib/node_modules/yarn/bin/yarn.js
+ yarn@1.22.5
added 1 package in 1.776s
```
   
### 2. create-react-app 설치하기
yarn이 설치 되었으면 create-react-app을 설치한다.
```terminal
yarn global add create-react-app
yarn global v1.22.5
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 🔨  Building fresh packages...

success Installed "create-react-app@3.4.1" with binaries:
      - create-react-app
✨  Done in 5.43s.
```
   
### 3. 리액트 앱 생성하기
```terminal
create-react-app dbs-example
zsh: command not found: create-react-app
```
이번에도 커맨드가 사전에 입력되지 않아 명령어를 찾지 못하는 문제가 발생함. 그냥 .bash_profile을 만들...어야 하는가.
#### Etc. .bash 파일에 yarn 커맨드 입력
```terminal
# yarn
export PATH="$(yarn global bin):$PATH
```

#### 다시 리액트 앱 생성
```terminal
create-react-app dbs-example
Creating a new React app in /Users/zeongyun/dbs-example.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template...

yarn add v1.22.5
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
info fsevents@2.1.2: The engine "node" is incompatible with this module. Expected version "^8.16.0 || ^10.6.0 || >=11.0.0". Got "8.10.0"
info "fsevents@2.1.2" is an optional dependency and failed compatibility check. Excluding it from installation.
[3/4] 🔗  Linking dependencies...
warning "react-scripts > @typescript-eslint/eslint-plugin > tsutils@3.17.1" has unmet peer dependency "typescript@>=2.8.0 || >= 3.2.0-dev || >= 3.3.0-dev || >= 3.4.0-dev || >= 3.5.0-dev || >= 3.6.0-dev || >= 3.6.0-beta || >= 3.7.0-dev || >= 3.7.0-beta".
[4/4] 🔨  Building fresh packages...
success Saved lockfile.
success Saved 23 new dependencies.
info Direct dependencies
├─ cra-template@1.0.3
├─ react-dom@16.13.1
├─ react-scripts@3.4.3
└─ react@16.13.1
info All dependencies
├─ @babel/helper-member-expression-to-functions@7.11.0
├─ @babel/plugin-syntax-typescript@7.10.4
├─ @babel/plugin-transform-flow-strip-types@7.9.0
├─ @babel/plugin-transform-runtime@7.9.0
├─ @babel/plugin-transform-typescript@7.11.0
├─ @babel/preset-typescript@7.9.0
├─ babel-preset-react-app@9.1.2
├─ cra-template@1.0.3
├─ eslint-config-react-app@5.2.1
├─ html-entities@1.3.1
├─ loglevel@1.7.0
├─ portfinder@1.0.28
├─ react-dev-utils@10.2.1
├─ react-dom@16.13.1
├─ react-error-overlay@6.0.7
├─ react-scripts@3.4.3
├─ react@16.13.1
├─ scheduler@0.19.1
├─ serialize-javascript@4.0.0
├─ sockjs@0.3.20
├─ spdy@4.0.2
├─ terser-webpack-plugin@2.3.8
└─ webpack-dev-server@3.11.0
✨  Done in 85.62s.

Initialized a git repository.

Installing template dependencies using yarnpkg...
yarn add v1.22.5
[1/4] 🔍  Resolving packages...
warning @testing-library/react > @types/testing-library__react > @types/testing-library__dom@7.5.0: This is a stub types definition. testing-library__dom provides its own type definitions, so you do not need this installed.
[2/4] 🚚  Fetching packages...
info fsevents@2.1.2: The engine "node" is incompatible with this module. Expected version "^8.16.0 || ^10.6.0 || >=11.0.0". Got "8.10.0"
info "fsevents@2.1.2" is an optional dependency and failed compatibility check. Excluding it from installation.
error @testing-library/dom@7.23.0: The engine "node" is incompatible with this module. Expected version ">=10". Got "8.10.0"
error Found incompatible module.
info Visit https://yarnpkg.com/en/docs/cli/add for documentation about this command.
`yarnpkg add @testing-library/react@^9.3.2 @testing-library/jest-dom@^4.2.4 @testing-library/user-event@^7.1.2` failed
```
에러 의미 확인 필요

### 4. 리액트 앱 구동하기
```terminal
cd dbs-example
yarn start
Compiled successfully!

You can now view dbs-example in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://172.30.1.27:3000

Note that the development build is not optimized.
To create a production build, use yarn build.
```
![result of yarn start](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/_posts/imgs/0829/yarn_start.png)

## 라이브러리 설치
`package.json`을 통해 필요한 라이브러리 미리 설치. GitHub(또는 자료실)에서 제공하는 package.json을 기준으로 라이브러리 설치
### 1. package.json 수정하기
```json
{
  "name": "dbs-snake",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "axios": "0.18.1",
    "enzyme": "^3.8.0",
    "enzyme-adapter-react-16.3": "^1.4.1",
    "moment": "^2.24.0",
    "next": "^8.1.0",
    "react": "^16.7.0",
    "react-dom": "^16.7.0",
    "react-redux": "^6.0.0",
    "react-router-dom": "^5.0.0",
    "react-scripts": "2.1.7",
    "react-test-renderer": "^16.7.0",
    "react-with-styles": "^3.2.1",
    "recompose": "^0.30.0",
    "redux": "^4.0.1",
    "redux-pack": "^0.1.5",
    "redux-thunk": "^2.3.0",
    "reselect": "^4.0.0",
    "selector-action": "^1.1.1"
  },
  "scripts": {
    "dev": "next",
    "predeploy": "yarn build-all",
    "deploy": "firebase deploy",
    "build-all": "yarn ssrbuild && yarn build-firebase",
    "build-firebase": "cd \"./functions\" && yarn --ignore-engines",
    "ssrbuild": "next build",
    "storybook": "start-storybook -p 9001 -c .storybook",
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "mockserver": "json-server --watch --delay 500 --port 4000 mock/create.js",
    "errorserver": "node mock/fake.js",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": [
    ">0.2%",
    "not dead",
    "not ie <= 11",
    "not op_mini all"
  ],
  "devDependencies": {
    "@babel/core": "7.5.5",
    "@babel/plugin-syntax-object-rest-spread": "^7.2.0",
    "@storybook/addon-actions": "^5.2.6",
    "@storybook/addons": "^5.2.6",
    "@storybook/react": "^5.2.6",
    "aphrodite": "^2.2.3",
    "babel-loader": "^8.0.5",
    "json-server": "^0.14.2",
    "node-sass": "^4.11.0",
    "react-with-styles-interface-aphrodite": "^5.0.1",
    "redux-devtools-extension": "^2.13.8",
    "sass-loader": "^7.1.0",
    "storybook-addon-jsx": "^7.1.13"
  }
}
```
### 2. package.json에 적힌 라이브러리 모두 설치하기
- 루트 폴더에서 명령어 실행
- 화면에 warning 메시지가 나타나는데, 이는 각 버전의 호환성에 대해서 제작 당시 명시된 버전과 다르다는 것을 알려주는 주의 내용이므로 무시해도 좋음.
```terminal
yarn
```
> 지속적인 node 버전 에러로 nodejs의 버전을 현 최신 버전인 14.9.0으로 설치 후 라이브러리 설치를 진행 함
> ```terminal
> nvm install 14.9.0
> nvm use 14.9.0
> yarn
> ```
> #### 라이브러리 설치 후 버전 회귀 및 환경 변수 파일 설정
> ```terminal
> nvm use 8.10.0
> ```
> ```env
> SKIP_PREFLIGHT_CHECK = true
> ```
> ```terminal
> yarn start
> ```

