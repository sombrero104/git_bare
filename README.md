# Git 설치
<br/>

### 1. 서버 git 설치 및 계정 생성
<pre>
yum install git

git config --global user.name "git"     // git 사용자 설정을 해준다.
git config --global user.email "glengleann@gmail.com"
git config --global color.ui "auto"
git config --global alias.st status   // git status 명령을 git st로 사용할 수 있게 할 수 있다.
git config --global --list            // 사용자 설정 확인
</pre>
</pre>
<pre>
adduser git   // git 계정 생성
passwd git	// git 암호 설정
su git  // 생성한 계정으로 전환
cd  // 사용자 git의 홈 디렉토리로 이동
</pre>
<pre>
ssh-keygen -t rsa     // rsa 암호화 방식으로 key 생성
Generating public/private rsa key pair.
Enter file in which to save the key (/home/git/.ssh/id_rsa):	// 그냥 Enter
Enter passphrase (empty for no passphrase):     // 그냥 Enter
Enter same passphrase again:      // 그냥 Enter
ls -al ~/.ssh     // /home/git/.ssh에 id_rsa, id_rsa.pub이 생성되었는지 확인
</pre><br/>

### 2. 내 PC git 설치
git 사용자 설정을 해준다.
<pre>
git config --global user.name "rey"
git config --global user.email "glengleann@gmail.com"
git config --global color.ui "auto"
git config --global alias.st status   // git status 명령을 git st로 사용할 수 있게 할 수 있다.
git config --global --list            // 사용자 설정 확인
</pre><br/>

### 3. 내 PC에서 SSH Key 생성
내 PC에서 만든 공개키를 git서버로 전송한다. <br/>
암호없이 git서버(git 계정으로.. git@192.168.59.2)로 접속하기 위해서다. <br/>
<pre>
cd ~
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/git/.ssh/id_rsa):	// 그냥 Enter
Enter passphrase (empty for no passphrase):				// 그냥 Enter
Enter same passphrase again:								// 그냥 Enter
ls -al ~/.ssh			// /home/[계정]/.ssh에 id_rsa, id_rsa.pub이 생성되었는지 확인
</pre><br/>

### 4. Public Key를 서버에 전송
<pre>
cd .ssh
scp id_rsa.pub git@192.168.59.2:/home/git/
</pre><br/>

### 5. 전송받은 Public Key 등록
<pre>
ssh git@192.168.59.2 			// ssh 원격 접속
cd
ls -al							// 전송한 id_rsa.pub 파일이 존재하는지 확인
cat id_rsa.pub >> .ssh/authorized_keys		// authorized_keys 파일로 저장
rm -rf id_rsa.pub							// id_rsa.pub 삭제
chmod 700 .ssh					// .ssh 디렉토리 권한 설정
chmod 600 .ssh/authorized_keys	// authorized_keys 파일 권한 설정
</pre><br/>

### 6. 이제 git 서버로 접속 테스트
<pre>
ssh git@192.168.59.2		// 암호를 물어보지 않는다면 성공
</pre><br/>

### 7. Git Repository 생성
<pre>
ssh root@192.168.59.2		// root 계정으로 접속
mkdir -p /git_repository/project.git	// /git_repository/project.git 디렉토리 생성
chown git:git /git_repository			// 소유권 변경
chown -R git /git_repository/project.git	// 소유권 변경
</pre>
<pre>
su git	// git 계정으로 전환
cd /git_repository/project.git	// 해당 디렉토리로 이동
git —bare init				// git bare 저장소 생성
cd		// git 계정의 홈디렉토리로 이동
ln -s /git_repository/project.git	// /home/git/project.git 심볼릭 링크 생성(project.git -> /git-repository/project.git)
</pre><br/>

### 8. 내 PC에서 최초의 clone, commit
<pre>
cd /Users/sombrero104/study_2020/git_study/
git clone git@192.168.59.2:project.git
</pre>
<pre>
cd project
echo 'Hello!' > readme.txt
git add readme.txt
git commit -m ‘.’
git push origin master
git status
git log
git pull	// 최신 내용으로 업데이트
</pre><br/>

### 9. 보안을 위해서 git 계정의 쉘 기능 제한
git 계정으로 접속한 유저들이 서버를 휘젓고 다니는 것을 막기 위해 쉘기능을 제한한다. <br/>
<pre>
root 계정으로 접속.
vi /etc/passwd

git 계정을 찾아서 /bin/bash 부분을 /usr/bin/git-shell로 변경한다.
git:x:1001:1001::/home/git:/bin/bash
=> git:x:1001:1001::/home/git:/usr/bin/git-shell

로그아웃 후 다시 git 계정으로 접속하면 아래와 같은 메시지를 출력하면서 접속이 제한된다.
(base) sombrero104ui-MacBookPro:project sombrero104$ ssh git@192.168.59.2
Last login: Sun Mar 15 00:32:02 2020 from 192.168.59.1
fatal: Interactive git shell is not enabled.
hint: ~/git-shell-commands should exist and have read and execute access.
Connection to 192.168.59.2 closed.
</pre><br/>
https://morningame.tistory.com/27 <br/>
<br/><br/><br/><br/>


### * 새로운 git 사용자(anton) 추가해보기

버추얼박스에 새로운 CentOS를 추가한 후, git을 설치한다.<br/>
<pre>
yum install -y net-tools
yum install -y git
</pre>
git 사용자 설정을 해준다.
<pre>
git config --global user.name "anton"
git config --global user.email "glengleann@gmail.com"
git config --global color.ui "auto"
git config --global alias.st status   // git status 명령을 git st로 사용할 수 있게 할 수 있다.
git config --global --list            // 사용자 설정 확인
</pre>
<pre>
cd ~
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/git/.ssh/id_rsa):	// 그냥 Enter
Enter passphrase (empty for no passphrase):				// 그냥 Enter
Enter same passphrase again:								// 그냥 Enter
ls -al ~/.ssh			// /home/[계정]/.ssh에 id_rsa, id_rsa.pub이 생성되었는지 확인
</pre>
<pre>
cd .ssh
scp id_rsa.pub git@192.168.59.2:/home/git/   // 위의 9번에서 보안을 위해 제한했던 쉘 기능을 다시 잠깐 풀어줘야 한다.
</pre>
<pre>
cat id_rsa.pub >> .ssh/authorized_keys  // authorized_keys 파일에 anton의 공개키를 추가한다.
</pre>
authorized_keys 파일을 열어보면 아래처럼 anton(rey@localhost.localdomain)의 공개키가 추가된 것을 확인할 수 있다.
<pre>
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDXoODORtSt4OwoWvLTC7BPfkaVHb0A3/MIcLfajtZ6YVP5TweEvmzZCh2YN7/oif2WP09c65ljaC1QkcZAMwgenQZ6sEYTHsh/Qp+/cEXBPIHxWMlf/FKxZnO/B6Vrishtf2d4sfOZc1QGUQmz/mhCRD/aBqS5lt4HEG5WMEhT8I5Bojw5FD37rGNAXafaAzs9UGOdJIvn9PAEqlZUH0aAGCMMfj+Jz7g0quZcKf7/HhtWlMBMXv2BZmT+LwEd/aIGtJ+2C/Ggaf0WkQi3AbHKs4TfwpEnkj2U1EibGfdMOWoe4cnpbF0RJjxXCcPMGcIzulEIFSf6p3S/E02upJp7 sombrero104@sombrero104ui-MacBookPro.local
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDs7mF5RFinScgp9CJVqYlfDFFx8pLPHy0/iF0dNqfzYR0aGyFBWgOCqNsGP1DPWfPtoGgCxWK10Zn8Mytn/jHjY5YibZBcgGX+muSynER1whwpia7sBDkVopV4QwlLWwFJFNEr+fma0cprAPskISb2KZZiKxsMlKRLP9QOg1+50VAnEKzhCiuFCoeIu9jgybVQ5we0euO4QPLXvsfcqBC0TzZM3nbzSMMB7d5hoIvkAatte747/Z5/3bbfYhDBbA9CTyC77ZIviiTBfJZ95yaf1te6HwpmvT5SzcQYNB+A2sID9KccLQQY9nS4YyA5au78oHwx/aveT0zGqNgDyrqd rey@localhost.localdomain
</pre>
프로젝트를 clone할 디렉토리를 만들고.. (/home/rey/study_2020/git_study/) <br/>
<pre>
cd /home/rey/study_2020/git_study/
git clone git@192.168.59.2:project.git
</pre>
파일 내용을 수정 후 커밋하면..
<pre>
vi readme.txt
git add readme.txt
git commit -m '.'
git push origin master
</pre>
anton이 커밋한 로그를 확인할 수 있다.
<pre>
[rey@localhost project]$ git log
commit 53910f291217a257db7c0f70ddb9bce0f81dd738
Author: anton <glengleann@gmail.com>
Date:   Sun Mar 15 11:52:33 2020 +0900

    .

commit 5c52a3c923973b71efce92ec401b7261a05bc32f
Author: rey <glengleann@gmail.com>
Date:   Sun Mar 15 11:08:46 2020 +0900

...
</pre>

<br/>
https://git-scm.com/book/ko/v2/Git-%EC%84%9C%EB%B2%84-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0 <br/>
<br/><br/><br/><br/>


### * 새로운 브랜치(release) 생성
remote 서버가 아닌 사용자 로컬에서 브랜치를 생성해서 remote 서버에 올려야 한다.
<pre>
git branch      // 브랜치 목록 확인
git branch -r   // remote 서버의 브랜치 목록 확인
git branch -a   // 사용자 로컬의 브랜치 목록 확인
</pre>
<pre>
// (1) 사용자 rey가 release 브랜치를 만들어서 remote에 push 함.
git checkout -b release     // release 브랜치를 생성 후 현재 HEAD를 release로 변경
git push origin release     // remote에 브랜치 반영

// (2) 사용자 anton이 remote에서 release 브랜치를 가져옴.
git fetch       // remote 정보를 업데이트
git checkout -t origin/release  // remote에 있는 release 브랜치 가져온 후 현재 HEAD를 release로 변경
</pre>
<br/><br/>

### * 이 외 Git 명령어
<pre>
git branch -d [브랜치명]   // 브랜치 삭제
</pre>
<br/><br/>




