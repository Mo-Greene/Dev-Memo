```
@Configuration  
@EnableWebSecurity  
@EnableMethodSecurity  
@RequiredArgsConstructor  
public class SecurityConfig {  
  
  
private final JwtFilter jwtFilter;  
private final CorsFilter corsFilter;  
private final JwtAccessDeniedHandler jwtAccessDeniedHandler;  
private final JwtAuthenticationEntryPoint jwtAuthenticationEntryPoint;  
  
@Bean  
public BCryptPasswordEncoder passwordEncoder() {  
return new BCryptPasswordEncoder();  
}  
  
@Bean  
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
http  
.csrf(CsrfConfigurer::disable)  
.httpBasic(HttpBasicConfigurer::disable)  
.cors(cors -> cors  
.configurationSource(corsFilter.corsConfigurationSource())  
)  
.sessionManagement(session -> session  
.sessionCreationPolicy(SessionCreationPolicy.STATELESS)  
)  
.exceptionHandling(exception -> exception  
.accessDeniedHandler(jwtAccessDeniedHandler)  
.authenticationEntryPoint(jwtAuthenticationEntryPoint)  
)  
.authorizeHttpRequests(request -> request  
  
//todo: h2 설정  
.requestMatchers(PathRequest.toH2Console()).permitAll()  
  
.anyRequest().permitAll()  
)  
// TODO: 2023/10/15 h2 설정 추가  
.csrf(csrf -> csrf.ignoringRequestMatchers(PathRequest.toH2Console ()))  
.headers(headers -> headers.frameOptions(HeadersConfigurer.FrameOptionsConfig::sameOrigin))  
  
.addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);  
  
return http.build();  
}  
}
```