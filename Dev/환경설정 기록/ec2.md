ec2 환경설정 순서 중요!

## 자바 설치
//설치방법
https://unit-15.tistory.com/114
//java corretto 11 버전 다운
https://docs.aws.amazon.com/ko_kr/corretto/latest/corretto-11-ug/generic-linux-install.html


## mysql 설치(no rds)
https://velog.io/@seungsang00/Ubuntu-%EC%9A%B0%EB%B6%84%ED%88%AC%EC%97%90-MySQL-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0
-> 도중에 mysql> use TESTDB; 를 통해 db를 바꿔주는것 생략

## EC2에서는 mysql을 다운받고 적용하려고 할때 회사에서와 똑같은 오류가 나왔다.
https://repost.aws/ko/questions/QUsxBfYkiVROCI5SBnEcHGPA/a-connection-to-the-ecc2-t2-micro-instance-could-not-be-established
일단 같은 오류를 물어본 구글 질문 -> 맞지는 않는것 같다.

https://repost.aws/questions/QUbI21xQGGSZuYHH3U8SIstQ/unable-to-connect-to-database-mysql-created-in-ec2-instance
위 블로그에 나름 자세하게 적혀져있음

## 포트포워딩
https://velog.io/@jinseoit/ec2-port-forward


## RDS 설치
https://velog.io/@nefertiri/AWS-RDS%EB%A1%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EB%A7%8C%EB%93%A4%EA%B8%B0

## EC2 timezone 설정
https://velog.io/@banjjoknim/EC2-%EC%84%9C%EB%B2%84-%EC%8B%9C%EA%B0%84-%EB%AC%B8%EC%A0%9C

## 보안그룹 설정 확인 필!
