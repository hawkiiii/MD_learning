创建SecurityConfig.java的class文件，

写一个类继承WebSecurityConfigurerAdapter，包含赋权，

```java
    @Configuration
    @Order(1)
    public static class AdminSecurityConfig extends WebSecurityConfigurerAdapter{
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.antMatcher("/admin/**")
                    .authorizeRequests()
                    .antMatchers("/css/**").permitAll()
                    .anyRequest()
                    .authenticated()

                    .and()
                    .formLogin()
                    .loginPage("/admin/login")
                    .defaultSuccessUrl("/admin/home")
                    .permitAll()

                    .and()
                    .logout()
                    .logoutUrl("/admin/logout")
                    .permitAll();
        }
    }
```



写一个bean类，注解@Bean。

```java
InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
manager.createUser(User
                   .withUsername("admin")
                   .password(encoder.encode("123456"))
                   .authorities("read","delete")
                   .build());
```

使用roles和authorities来赋权或者给角色。

.hasRoles()
.authorities("read","delete")

