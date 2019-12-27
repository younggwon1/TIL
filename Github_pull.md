# Github에 있는 코드 가져오기

``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~
$ mkdir house

HPE@DESKTOP-STUDENT1 MINGW64 ~
$ cd house

HPE@DESKTOP-STUDENT1 MINGW64 ~/house
```



``` shell
HPE@DESKTOP-STUDENT1 MINGW64 ~/house
$ git clone https://github.com/younggwon1/TIL.git (처음만 git clone 그 후에는 git pull)
Cloning into 'TIL'...
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 10 (delta 3), reused 8 (delta 1), pack-reused 0
Unpacking objects: 100% (10/10), done.

HPE@DESKTOP-STUDENT1 MINGW64 ~/house
$ ls
TIL/

HPE@DESKTOP-STUDENT1 MINGW64 ~/house
$ cd TIL

HPE@DESKTOP-STUDENT1 MINGW64 ~/house/TIL (master)
$ LS
00_markdown_basic.md  01_Github_TIL.md  Git.md  README.md

HPE@DESKTOP-STUDENT1 MINGW64 ~/house/TIL (master)
$ touch linux.md

HPE@DESKTOP-STUDENT1 MINGW64 ~/house/TIL (master)
$ ls
00_markdown_basic.md  01_Github_TIL.md  Git.md  linux.md  README.md

HPE@DESKTOP-STUDENT1 MINGW64 ~/house/TIL (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        linux.md

nothing added to commit but untracked files present (use "git add" to track)

HPE@DESKTOP-STUDENT1 MINGW64 ~/house/TIL (master)
$ git add linux.md

HPE@DESKTOP-STUDENT1 MINGW64 ~/house/TIL (master)
$ git commit -m "ADD linux.md"
[master 6470be5] ADD linux.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 linux.md

HPE@DESKTOP-STUDENT1 MINGW64 ~/house/TIL (master)
$ git push origin master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 307 bytes | 307.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/younggwon1/TIL.git
   7c6fe64..6470be5  master -> master

HPE@DESKTOP-STUDENT1 MINGW64 ~/house/TIL (master)
$ cd ..

HPE@DESKTOP-STUDENT1 MINGW64 ~/house
$ cd ..

HPE@DESKTOP-STUDENT1 MINGW64 ~
$ cd TIL

HPE@DESKTOP-STUDENT1 MINGW64 ~/TIL (master)
$ git pull origin master
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/younggwon1/TIL
 * branch            master     -> FETCH_HEAD
   7c6fe64..6470be5  master     -> origin/master
Updating 7c6fe64..6470be5
Fast-forward
 linux.md | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 linux.md
```



