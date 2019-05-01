# 9. Spring MVC와 Web Security의 통합
## 1. 프로젝트 구성
- Security
- DevTools
- Lombok
- JPA
- MySQL
- Thymeleaf
- Web

### 추가 라이브러리
#### Thymeleaf-layout
```xml
<dependency>
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>
</dependency>
```

#### Querydsl
```xml
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-apt</artifactId>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-jpa</artifactId>
</dependency>
```

#### Thymeleaf-security
```xml
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity5</artifactId>
</dependency>
```

### Querydsl을 활용하기 위한 <plugin> 추가
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
        <plugin>
            <groupId>com.mysema.maven</groupId>
            <artifactId>apt-maven-plugin</artifactId>
            <version>1.1.3</version>
            <executions>
                <execution>
                    <goals>
                        <goal>process</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>target/generated-sources/java</outputDirectory>
                        <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### 기존 프로젝트 통합
#### 1. application.properties
```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/jpa_ex?useSSL=false
spring.datasource.username=jpa_user
spring.datasource.password=jpa_user

# 스키마 생성(create)
#spring.jpa.hibernate.ddl-auto=create
spring.jpa.hibernate.ddl-auto=update
# DDL 생성 시 데이터베이스 고유의 기능을 사용하는가?
spring.jpa.generate-ddl=true
# 실행되는 SQL문을 보여줄 것인가?
spring.jpa.show-sql=true
# 데이터베이스는 무엇을 사용하는가?
spring.jpa.database=mysql
# 로그 레벨
logging.level.org.hibernate=info
# SQL 파라메터 값까지 보기
#logging.level.org.hibernate.type.descriptor.sql=trace
# MySQL 상세 지정
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect

# Thymeleaf 캐싱 사용안함
spring.thymeleaf.cache=false
logging.level.org.springframework.web=debug
logging.level.org.springframework.security=debug
logging.level.me.freelife=debug
```

#### 2. 패키지 복사
#### 3. resources의 static, templates 폴더 복사
#### 4. maven compile 수행하여 Querydsl QWebBoard 생성
#### 5. 프로젝트 실행
security 기본 user 계정으로 로그인 '/board/list' 화면 출력확인

### 시큐리티 설정
#### domain 패키지
Member, MemberRole 추가

#### persistence 패키지
MemberRepository 추가

#### controller 패키지
LoginController, MemberController 추가

#### security 패키지 복사
