# Git

### SCM (source code management)

### VCS (version control system)

---



코드 관리 도구, 변경사항을 관리



## 폴더(디렉토리) 로 관리



## version 확인

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL
$ git --version
git version 2.24.0.windows.2
```



## git 시작

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL
$ git init
Initialized empty Git repository in C:/Users/HPE/TIL/.git/
```



## (. git)을 가지고 코드를 관리

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ ls -a
./  ../  .git/
```



## git 상태 확인하기

``` shell
git에게 상태를 물어본다.(매번)
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git status
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```



## 파일 옮기기

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git status
On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        00_markdown_basic.md
nothing added to commit but untracked files present (use "git add" to track)
```



## 해당 파일 올리기 (사진대에 올리기)

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git add 00_markdown_basic.md

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git status
On branch master
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   00_markdown_basic.md
```



## 본인 설정하기

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git config --global user.email
아이디
git config --global user.email 이메일 주소

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git config --global user.name
이름
```



## 해당 파일 commit하기 (사진 찍기)

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git commit
-> vi editor에서 first commit을 입력
[master (root-commit) 9ea0e87] first commit
 1 file changed, 168 insertions(+)
 create mode 100644 00_markdown_basic.md
 
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git status
On branch master
nothing to commit, working tree clean -> 깔끔하게 마무리 됨
```

---



## README.md 파일 생성

- 현재 프로젝트의 설명서와 같다.
- 따라서 만들어야한다.

``` shell
파일을 만들 때 마다 README를 생성해야한다. (현재 프로젝트의 설명서와 같음)
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
nothing added to commit but untracked files present (use "git add" to track)

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git add README.md

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   README.md

이전 파일과 현재 파일의 차이를 알려주는 것(사진대에 올라가 있는 파일들)
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git diff --staged
diff --git a/README.md b/README.md
new file mode 100644
index 0000000..4649780
--- /dev/null
+++ b/README.md
@@ -0,0 +1,3 @@
+# TIL
+
+Today | Learned
\ No newline at end of file

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git commit -m "ADD README.md"
[master cbd2cc3] ADD README.md
 1 file changed, 3 insertions(+)
 create mode 100644 README.md

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git log
commit ~~~ (HEAD -> master)
Author: Younggwon <dlrjsapdlf622git config --global user.email 이메일 주소git config --global user.email 아이디>
Date:   Fri Dec 27 14:51:43 2019 +0900

    ADD README.md

commit ~~~
Author: Younggwon <dlrjsapdlf622git config --global user.email 이메일 주소git config --global user.email 아이디>
Date:   Fri Dec 27 14:41:35 2019 +0900

    first commit
```

---





## 이전 버전으로 돌려보기

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git log --oneline
cbd2cc3 (HEAD -> master) ADD README.md
9ea0e87 first commit

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git checkout 9ea0e87

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL ((9ea0e87...))
$ git log
commit ~~~ (HEAD)
Author: 이름 <아이디git config --global user.email 이메일 주소git config --global user.email 아이디>
Date:   Fri Dec 27 14:41:35 2019 +0900

    first commit
```



## 원래 버전으로 돌아오기

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL ((9ea0e87...))
$ git checkout master
Previous HEAD position was 9ea0e87 first commit
Switched to branch 'master'

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ ls
00_markdown_basic.md  README.md

```



## GitHub에 코드 올리기

### 1. 설정하기

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git remote add origin github주소

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git remote
origin

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git remote -v
origin  github주소 (fetch)
origin  github주소 (push)
```

### 2. 코드 올리기

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git push origin master
-> 로그인하기 (github)
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 2.24 KiB | 573.00 KiB/s, done.
Total 6 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To github주소
 * [new branch]      master -> master
```





