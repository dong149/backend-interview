# Spring

#### Spring 과 Spring Boot의 차이?

<details>
<summary style="color:skyblue">정답보기</summary>
<Blockquote>
<br>

- Embed Tomcat 을 사용하기 때문에, 따로 Tomcat을 설치하거나, 매번 버전관리를 해줄 필요가 없다.
- 기존에는 스프링을 사용할 때 버전까지 명시하고, 버전에 맞는 설정을 해주어야 했지만, 스프링 부트는 버전 관리를
  스프링 부트에 의해 관리된다. 따라서, 예를들어 Spring-boot-starter-web을 사용하면 종속된 모든 라이브러리를 알맞게 찾아서
  함께 가져오기 때문에 종속성이나 호환 버전에 대해 신경 쓸 필요가 없다.
- Auto Configuration 지원
- Actuator 기능 지원

https://ssoco.tistory.com/66

</Blockquote>
</details>

#### @Configuration안에 @Bean을 사용해야 하는 이유

<details>
<summary style="color:skyblue">정답보기</summary>
<Blockquote>
<br>

스프링에서는 일반적으로, 컴포넌트 스캔을 사용해 자동으로 빈을 등록하는 방법을 이용한다.

하지만, @Bean를 사용해 수동으로 빈을 등록해야 하는 경우도 있다.
대표적으로 다음과 같은 경우에 @Bean으로 직접 빈을 등록해준다.

1. 개발자가 직접 제어가 불가능한 라이브러리를 활용할 때
2. 어플리케이션 전범위적으로 사용되는 클래스를 등록할 때
3. 다형성을 활용하여 여러 구현체를 등록해주어야 할 때

@Bean을 이용한 메소드는 스프링 빈 안에만 구현되어 있으면 모두 동작하긴 한다.
하지만, 스프링은 @Bean은 반드시 @Configuration을 활용하도록 갖오하는데, 그 이유는 특별한 부가 기능이 적용되기 때문이다.

@Configuration에는 @Component가 붙어있어서 @Configuration이 붙어있는 클래스 역시 스프링의 빈으로 등록된다.

그럼에도 불구하고 스프링이 @Configuration을 따로 만든 이유는 CGLib으로 프록시 패턴을 적용해 수동으로 등록하는 스프링 빈이 반드시 싱글톤으로 생성됨을 보장하기 위해서다.

실수로 빈을 생성하는 메소드를 여러 번 호출하면, 아래에서 testResources 메소드와 같이 불필요하게 여러 개의 빈이 생성될 수 있다.

```java
@Configuration
public class MyBean {
	@Bean
	public TestResource testResource() {
	  return new TestResource();
	}

	@Bean
	public FirstBean firstBean() {
	  return new FirstBean(testResource());
	}

	@Bean
	public SecondBean secondBean() {
	  return new SecondBean(testResource());
	}
}
```

이를 스프링에서는

```java
@Configuration
public class MyBeanProxy extends MyBean {
  private Object source;

	@Bean
	public TestResource testResource() {
    if(testResource == null) {
      source = super.testResource();
    }

	  return source;
	}

	@Bean
	public FirstBean firstBean() {
	  return super.firstBean();
	}

	@Bean
	public SecondBean secondBean() {
	  return super.secondBean();
	}
}
```

와 같은 식으로 프록시 패턴을 사용하여 해결한다. (예시일 뿐이다.)

https://mangkyu.tistory.com/234

</Blockquote>
</details>

#### @Bean, @Configuration vs @Component 차이?

<details>
<summary style="color:skyblue">정답보기</summary>
<Blockquote>
<br>

### @Bean, @Configuration

- 수동으로 스프링 컨테이너에 빈을 등록함.
- 개발자가 직접 제어가 불가능한 외부 라이브러리 등을 Bean으로 만들려고 할 때, 사용한다.

```java
@Configuration
public class RestTemplateConfig {
  @Bean
  public RestTemplate restTemplate(){
    return new RestTemplate();
  }
}
```

- 유지보수성을 높이기 위해 애플리케이션 전범위적으로 사용되는 클래스나 다형성을 활용하여 여러 구현체를 빈으로 등록할 때 사용.

```java
@Configuration
public class RestTemplateConfig {
  @Bean
  public RestTemplate testApiRestTemplate(){
    return new RestTemplate();
  }

  @Bean
  public RestTemplate testApi2RestTemplate(){
    return new RestTemplate();
  }
}
```

### @Component

- 자동으로 스프링 컨테이너에 빈을 등록하는 방법
- 스프링의 @ComponentScan 기능이 @Component 어노테이션이 있는 클래스를 자동으로 찾아서 빈으로 등록한다.

https://mangkyu.tistory.com/75

</Blockquote>
</details>
