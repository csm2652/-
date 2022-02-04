# ExceptionHandler

### 인증/ 인가 API - ExceptionTranslationFilter

+ AuthenticationException

  + 인증 예외 처리
    1. AuthenticationEntryPoint 호출
    2. 인증예외가 발생하기 전의 오청 정보를 저장
    3. RequestCache: 사용자의 이전 요청 정보를 세션에 저장하고 이를 꺼내 오는 캐시 메카니즘
    4. SavedRequest: 사용자가 요청했던 request 파라미터 값들, 그 당시의 헤더값들 등이 저장
         
  
  
+ AccessDeniedException
  + 인가 예외 처리
   
    1. AccessDeniedHandler에서 예외 처리하도록 제공
___
### http.exceptionHandling()
+ 예외처리 기능이 작동함
```
protected void configure(HttpSecurity http) throws Exception {
	 http.exceptionHandling() 					
		.authenticationEntryPoint(authenticationEntryPoint())  // 인증실패 시 처리
		.accessDeniedHandler(accessDeniedHandler())// 인가실패 시 처리
}

```
+ authenticationEntryPoint
   + 인증 실패시 작동
+ accessDeniedHandler
  + 인가 실패시 작동