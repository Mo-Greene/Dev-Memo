#참고 


1. 서비스 코드에 Response 등 응답객체 반환 금지 -> 테스트 코드 작성시 진행하기 어려운 부분이 있음
2. 테스트 코드 작성 시 Controller는 Mock테스트, 그 외 Junit 단위테스트로
3. 패키지 구조는 api와 global로 나눈다.
	1. api의 패키지는 도메인별로 나눈다.(기능별 x)
	2. exception의 경우 global 패키지의 도메인으로 나눈다.(상황에 따라 변경)
	3. 디자인패턴을 적용시켜야 할 경우 Service 패키지를 작성하여 그안에 보관한다.
	4. api 패키지 내에 특정 도메인만 사용하게 되는 util 패키지를 만들지 않는다.(생길 경우 디자인패턴으로 보관)
	5. util의 경우 특정 도메인의 컴포넌트를 DI 하지 않는다. (ex: private final MemberRepository(X))
	6. dto패키지 안에는 request, response을 패키지로 나누고 어플리케이션 내부에서 사용할 dto의 경우 dto패키지 안에 넣도록 하자
4. 메서드 명으로 get, set 등의 prefix를 사용하지 않는다.
5. 노출되어선 안될 내용이 없다면 Entity로 반환해도 문제없다.(ex: 약관 등)


### JPA
- join 및 단방향관계, 양방향관계는 매우 신중하게 결정(특히 삭제되는 데이터를 주의하면서 진행) 애지간하면 join을 걸지 않고 queryDsl을 사용해서 사용한다.
	- [유용한 답변](https://www.inflearn.com/questions/39769/%EB%B6%80%EB%AA%A8-%EC%9E%90%EC%8B%9D%EA%B4%80%EA%B3%84%EC%97%90%EC%84%9C-%EB%B6%80%EB%AA%A8-%EC%82%AD%EC%A0%9C%EC%8B%9C-set-null%EB%B0%A9%EB%B2%95%EC%97%90-%EB%8C%80%ED%95%B4%EA%B6%81%EA%B8%88%ED%95%A9%EB%8B%88%EB%8B%A4)
- 