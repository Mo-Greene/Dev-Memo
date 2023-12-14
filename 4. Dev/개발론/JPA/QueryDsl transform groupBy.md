#jpa 

[querydsl transform](https://velog.io/@songunnie/Spring-QueryDSL-transform%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)

```
주입 필요
import static com.querydsl.core.group.GroupBy.groupBy;
```

[트랜스폼 기본 설정](https://velog.io/@dktlsk6/QueryDSL-transform-%EC%97%90%EB%9F%AC)

```
//사용예시
@Override  
public List<MemberApplyDetailResponse> findMemberApplyDetail(Long id) {  
  
	return queryFactory  
		.selectFrom(member)  
		.leftJoin(memberLicense).on(member.id.eq(memberLicense.member.id))  
		.where(member.id.eq(id))  
		.orderBy(memberLicense.id.asc())  
		.transform(
	groupBy(member.id).list(Projections.constructor(MemberApplyDetailResponse.class,  
		member.id.as("id"),  
		member.isApproval.as("isApproval"),  
		member.createAt.as("createAt"),  
		member.email.as("email"),  
		member.store.as("store"),  
		member.address.as("address"),  
		member.addressDetails.as("addressDetails"),  
		member.phone.as("phone"),  
		member.memberCategory.categoryName.as("categoryName"),  
		member.bizLicenseNumber.as("bizLicenseNumber"),  
		list(Projections.constructor(ApplyLicenseResponse.class,  
			memberLicense.id.as("licenseId"),  
			memberLicense.licenseName.as("licenseName")  
					)  
				)  
			)  
		)  
	);  
}
```