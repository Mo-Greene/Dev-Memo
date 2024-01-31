#docker #jenkins

[참고](https://goddaehee.tistory.com/259)
[웨지 참고](https://sihyung92.oopy.io/e5300d92-1a4e-40f4-b927-a93b2bbb17d2#47645877-e087-4a3d-92d4-d8a638c867d2)
[ec2 방화벽 허용](https://velog.io/@penrose_15/github-webhook-ip)

기본 포트 변경
```
docker run -itd --env JENKINS_OPTS=--httpPort=7070 -p 7070:7070 -v ~/jenkins:/var/jenkins_home --name jenkins jenkins/jenkins:lts
```

몇가지 기본설정 후
비밀번호 확인, 계정생성

config에서 github를 확인
![[Pasted image 20240131084038.png]]


새로운 item 생성(freestyle)
General
- 설명
- github project (내 github 주소)

소스코드 관리
- Git
	- Repository Url
	- 등록한 credentials

지금 빌드 실행!
성공

Build Steps
- Execute shell 커맨드라인 작성

JAR_NAME 을 지정하고 빌드 재시도
성공

Publish over SSH 등록
플러그인 설치
System 환경설정에서 Publish over SSH 설정 시작
- Key pem 키 등록
- SSH Server
	- Name : 접속할 SSH 이름
	- Hostname : ip address(같은 Vpc의 경우 private address 가능)
	- Username : ubuntu
	- Remote Directory : 가장 기본 dir (ubuntu의 경우 : /home/ubuntu)

빌드 아이템 이동 후 빌드 후 조치 : Send build artifacts over SSH

Exec command 작성 필요!

``` shell
//실서버 shell
echo ">> check pid"
CURRENT_PID=$(pgrep app.jar)

echo ">> 현재 실행되고 있는 PID : $CURRENT_PID"

if [ -z "$CURRENT_PID" ]; then
  echo "> 현재 구동 중인 애플리케이션이 없습니다."
else
  echo ">> KILL PID  : $CURRENT_PID"
  kill -9 $CURRENT_PID
  sleep 10
fi
echo ">> 어플리케이션 배포"
JAR_PATH=$(ls -t /home/ubuntu/app/app.jar | head -1)

sudo nohup java -jar -DServer.port=80 ${JAR_PATH} >> /home/ubuntu/log/app.log &
```



기존 타임아웃 에러
``` shell
SSH: Disconnecting configuration [Dev-Mo] ...
ERROR: Exception when publishing, exception message [Exec timed out or was interrupted after 120,001 ms]
Build step 'Send build artifacts over SSH' changed build result to UNSTABLE
Finished: UNSTABLE
```

![[Pasted image 20240131115848.png]]
에러 수정
![[Pasted image 20240131115746.png]]

또 swapfile을 꼭 만들어서 서버의 메모리 용량을 늘려주도록 하자
빌드가 진행되지 않는다;;
![[Pasted image 20240131125450.png]]
7분째 ing.. 하다가 짜증나서 그만뒀다.


``` shell
pipeline {
    agent any

    stages {
        stage('clone') {
            steps {
                git credentialsId: 'github-access', 
                url: 'https://github.com/Mo-Greene/Today-Algorithm.git'
            }
        }
        
        stage('build') {
            steps {
                sh'''
                    echo ">> build start"
                    ./gradlew clean build
                '''
                
            }
        }
        
        stage('publish on ssh') {
            steps {
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'Dev-Mo',
                            transfers: [
                                sshTransfer(
                                    cleanRemote: false,
                                    excludes: '',
                                    execCommand: 'sh /home/ubuntu/script/server.sh',
                                    execTimeout: 120000, 
                                    flatten: false, 
                                    makeEmptyDirs: false, 
                                    noDefaultExcludes: false,
                                    patternSeparator: '[, ]+',
                                    remoteDirectory: '/app', 
                                    remoteDirectorySDF: false, 
                                    removePrefix: 'build/libs', 
                                    sourceFiles: 'build/libs/app.jar'
                                )
                            ], 
                            usePromotionTimestamp: false,
                            useWorkspaceInPromotion: false,
                            verbose: true
                        )
                    ]
                )
            }
        }
    }
}
```