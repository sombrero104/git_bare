# Git 설치
<br/>

### 1. 서버 git 설치 및 계정 생성
<pre>
yum install git
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
<br/>

### 3. 내 PC에서 SSH Key 생성
내 PC에서 만든 공개키를 git서버로 전송한다. <br/>
암호없이 git서버(git 계정으로.. git@192.168.59.2)로 접속하기 위해서다. <br/>
<br/>
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

<br/>
https://morningame.tistory.com/27 <br/>
<br/><br/>
