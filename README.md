# Introduction
A spring cloud gateway with Spring Security. (register with eureka).
### IDE
Eclipse 2021.06
### Build
Maven
### Language
JAVA
### Run
JAR (JDK 11)
### Version
Spring Cloud 2020.0.4

# Design
Client should request token before access. (token is generated by authService)

# 測試範例

白名單設定於：WhiteListConfig

	[authService] /auth/login（要經 AuthFilter 驗證 token, 不用經 UserFilter 確認登入狀態）：
	
	取得 token {/auth/register} --> 攜帶 token 進行登入 {/auth/login} -- (成功) --> 紀錄至 Redis --> HttpStatus.OK
												|
												|
												--- -- --- -- --- -- --- (失敗) --> HttpStatus.UNAUTHORIZED
						
	[memberService] /member/test（需 UserFilter 確認登入狀態）：
	
	[GET] /member/test --> 確認登入狀態及權限 (Redis) -- (允許訪問) --> Return 測試成功 --> HttpStatus.OK
									|
									|
									-- --- -- --- -- --- (尚未登入) --> throw new Exception("User not found or unavailable.")
									|
									|
									-- --- -- --- -- --- (權限不足) --> throw new Exception("User role not allowed.")