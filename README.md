# sivanWu0222.github.io.source

博客源码 博客自动化部署 hexo博客

步骤如下：
1. appveyor 添加项目
2. 在自己的项目本地文件中**添加appveyor.yml配置文件**，同时配置**密钥**
3. 在appveyor中配置项目的环境变量
4. 将变动push到对应的github的仓库中，appveyor探查到仓库变动之后，将会自动渲染并部署变动到STATIC_SITE_REPO 指定的仓库中


## 设置AppVeyor
> 添加好appveyor.yml之后，再到Appveyor portal设置以下四个变量。STATIC_SITE_REPO就是Content Repo的地址，TARGET_BRANCH就是你Content Repo的branch，一般默认就是master，GIT_USER_EMAIL和GIT_USER_NAME就是你GitHub账号的信息。


STATIC_SITE_REPO ： 就是hexo渲染后存放的仓库。一般都是username.github.io
TARGET_BRANCH: 就是存放源代码的仓库的分支。默认是master
GIT_USER_EMAIL: GitHub的邮箱地址
GIT_USER_NAME: GitHub的用户名



## appveyor.yml配置文件

```
clone_depth: 5

environment:
  access_token:
    secure: 7AzX+wI/I7gTU6LmboXo1zyCb7Qw+1IGwUUrFW31ayFsTlvN/aVh+k8FWWI1PlNB


install:
  - node --version
  - npm --version
  - npm install
  - npm install hexo-cli -g

build_script:
  - hexo generate

artifacts:
  - path: public

on_success:
  - git config --global credential.helper store
  - ps: Add-Content "$env:USERPROFILE\.git-credentials" "https://$($env:access_token):x-oauth-basic@github.com`n"
  - git config --global user.email "%GIT_USER_EMAIL%"
  - git config --global user.name "%GIT_USER_NAME%"
  - git clone --depth 5 -q --branch=%TARGET_BRANCH% %STATIC_SITE_REPO% %TEMP%\static-site
  - cd %TEMP%\static-site
  - del * /f /q
  - for /d %%p IN (*) do rmdir "%%p" /s /q
  - SETLOCAL EnableDelayedExpansion & robocopy "%APPVEYOR_BUILD_FOLDER%\public" "%TEMP%\static-site" /e & IF !ERRORLEVEL! EQU 1 (exit 0) ELSE (IF !ERRORLEVEL! EQU 3 (exit 0) ELSE (exit 1))
  - git add -A
  - if "%APPVEYOR_REPO_BRANCH%"=="master" if not defined APPVEYOR_PULL_REQUEST_NUMBER (git diff --quiet --exit-code --cached || git commit -m "Update Static Site" && git push origin %TARGET_BRANCH% && appveyor AddMessage "Static Site Updated")
```

**注意把自己的secure对应的密钥换成自己的密钥**

## 参考
[参考](https://www.jianshu.com/p/db9a95078b16)

