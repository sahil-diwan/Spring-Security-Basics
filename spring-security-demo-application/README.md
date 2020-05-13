# Spring Security Basic Application

Create a spring boot project and include dependencies
  1. Starter web
  2. Spring Security
  
  As we add the Spring Security ,spring boot adds a default behavior
   ```
    1.	Adds mandatory authentication for all those URL’s.
    2.	Adds login form.
    3.	Handles login error.
    4.	Creates a user and sets a default password.
   ```
  ```
    By default the user id and the password is 
	  User id : User
	  Password will be generated in the console.
    
    But we can edit this user id and credentials also by editing the application.properties
    Spring.security.user.name= foo
    Spring.security.user.password=foo

 ```
  Now this default behavior is handled by AuthenticationManager of the spring security we can edit the
  behavior of the authentication by getting hold of AuthenticationManager by using AuthenticationManagerBuilder
  
  # Home Resource
  
  ```
    package com.sahildiwan.springsecurity.springsecuritydemoapplication;



    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class HomeResource {

        @GetMapping("/")
        public String home() {
            return ("<h1>Welcome</h1>");
        }

        @GetMapping("/user")
        public String user() {
            return ("<h1>Welcome User</h1>");
        }

        @GetMapping("/admin")
        public String admin() {
            return ("<h1>Welcome Admin</h1>");
        }
    }

  ```
  
  # Security Configuration
  
  ```
   package com.sahildiwan.springsecurity.springsecuritydemoapplication;

  import org.springframework.context.annotation.Bean;
  import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
  import org.springframework.security.config.annotation.web.builders.HttpSecurity;
  import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
  import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
  import org.springframework.security.crypto.password.NoOpPasswordEncoder;
  import org.springframework.security.crypto.password.PasswordEncoder;

  @EnableWebSecurity
  public class SecurityConfiguration extends WebSecurityConfigurerAdapter{

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
      // TODO Auto-generated method stub
      auth.inMemoryAuthentication()
        .withUser("sahil")
        .password("sahil")
        .roles("USER")
        .and()
        .withUser("rohan")
        .password("rohan")
        .roles("ADMIN");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
      // TODO Auto-generated method stub
      http.authorizeRequests()
      .antMatchers("/admin").hasRole("ADMIN")
        .antMatchers("/user").hasAnyRole("USER","ADMIN")
        .antMatchers("/").permitAll()
        .and().formLogin();
    }

    @Bean
    public PasswordEncoder getPasswordEncoder(){
      return NoOpPasswordEncoder.getInstance();
    }

  }
  
  ```
