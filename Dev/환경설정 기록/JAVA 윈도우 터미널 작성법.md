1. 윈도우 터미널에서 git bash 사용
2. java -version 명령어를 사용해도 계속 환경변수로 설정한 jdk 버전이 나오지 않음
3. git bash 설정 제대로 하는 방법

```
//git bash java 설정 현재 위치
echo $PATH
```

```
//jdk 디렉토리 경로 파악 후 java PATH 추가
export PATH=`jdk파일 경로 c://programefile\corettojdk\등등\bin`:$PATH

//위와같이 설정할 경우 echo $PATH 의 값 설정한 경로로 바뀌게 된다. (bin 중요)
```

11과 17을 같이 쓰고있다면 주의해서 사용해야됨