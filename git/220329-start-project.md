# Shell command
### cd, mkdir, ls, pwd, touch, mv, cp, rm, cat, vi

- rm -r(f) [dir]: 하위에 있는 디렉토리와 안에 있는 파일 모두 지우기
- ls -a: 숨김 파일 모두 표시
<br>

c.f. delete(물리적인 삭제) vs remove(논리적인 삭제)
- 컴퓨터 내부에서 파일 삭제는 remove
- 포렌식 검사 시 삭제파일을 검출해낼 수 있는 이유

<br>

# Vim command
### i, ESC, dd, yy, p , u
- yank: paste와 같은 의미

## Command mode
### q, q!, w, wq, :{number}

<br>

# Git
VCS(Version Controll System) <br>
== SCM (Source Code Management)
<br>

c.f. SCM(Software Configuration Management: 형상관리)

<br>

## Git Object
- Blob, Tree, Commit

## git != github
- git은 도구, github는 웹 서비스

## Git command

- `$ git init`: Bottom-up 방식의 git 설정
```git
$ mkdir {directory}
$ git init
$ git remote add origin {.git 주소}
# 파일 생성
$ git add README.md
$ git commit
$ git push -u origin main
```
<br>

** -u: 원격저장소에 있는 branch와 로컬저장소에 있는 branch를 같게 함 ** <br>
** 2번째 push는 `$ git push origin main` **
<br>

- `$ git clone`: Top-down 방식의 git 설정
```git
$ git clone {.git 주소}
$ git add {파일}
$ git commit
$ git push
```
<br>

## Conventinal Commits
prefix 습관화!
- feat, fix, docs, test, conf, build, ci
- e.g. feat: Create Adder.java
