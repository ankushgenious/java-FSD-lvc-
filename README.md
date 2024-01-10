# java-FSD-lvc-

PHASE 3
# project 1: Searching for a Specific User and Updating the User Information :
Searching for a Specific User and Updating the User Information :
Source code:
UserDao.java :
public interface UserDao { 
User getUserById(int userId); 
void updateUser(User user); 
} 
HibernateUserDao.java :
@Repository
public class HibernateUserDao implements UserDao { 
@Autowired
private SessionFactory sessionFactory; 
@Override
public User getUserById(int userId) { 
Session session = sessionFactory.getCurrentSession(); 
return session.get(User.class, userId); 
} 
@Override
public void updateUser(User user) { 
Session session = sessionFactory.getCurrentSession(); 
session.update(user); 
} 
} 
UserService.java :
public interface UserService { 
User getUserById(int userId); 
void updateUser(User user); 
} 
ServiceImpl.java :
@Service
public class UserServiceImpl implements UserService { 
@Autowired
private UserDao userDao; 
@Override
public User getUserById(int userId) { 
return userDao.getUserById(userId); 
} 
@Override
public void updateUser(User user) { 
userDao.updateUser(user); 
} 
} 
UserController.java :
@Controller 
@RequestMapping("/user") 
public class UserController { 
@Autowired
private UserService userService; 
@GetMapping("/edit") 
public String showEditForm(@RequestParam("userId") int userId, Model
model) { 
User user = userService.getUserById(userId); 
if (user == null) { 
// Handle invalid user ID 
return "errorPage"; 
} 
model.addAttribute("user", user); 
return "editUser"; 
} 
@PostMapping("/update") 
public String updateUser(@ModelAttribute("user") User user) { 
userService.updateUser(user); 
return "confirmation"; 
} 
}
enterUserId.jsp :
<%@ page language="java" contentType="text/html; charset=UTF-8" 
pageEncoding="UTF-8" %> 
<!DOCTYPE html> 
<html> 
<head> 
<title>Enter User ID</title>
</head> 
<body> 
<h1>Enter User ID:</h1>
<form action="${pageContext.request.contextPath}/user/edit" method="get">
<input type="number" name="userId" required>
<button type="submit">Edit</button>
</form>
</body> 
</html> 
editUser.jsp :
<%@ page language="java" contentType="text/html; charset=UTF-8" 
pageEncoding="UTF-8" %> 
<!DOCTYPE html> 
<html> 
<head> 
<title>Edit User</title>
</head> 
<body> 
<h1>Edit User</h1>
<form action="${pageContext.request.contextPath}/user/update"
method="post">
<input type="hidden" name="userId" value="${user.userId}">
<label>Username:</label>
<input type="text" name="username" value="${user.username}" required><br>
<label>Email:</label>
<input type="email" name="email" value="${user.email}" required><br>
<button type="submit">Update</button>
</form>
</body> 
</html> 
confirmation.jsp :
<%@ page language="java" contentType="text/html; charset=UTF-8" 
pageEncoding="UTF-8" %> 
<!DOCTYPE html> 
<html> 
<head> 
<title>Update Confirmation</title>
</head> 
<body> 
<h1>Update Successful</h1>
<p>User details have been updated successfully.</p>
</body> 
</html> 
pom.xml :
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
http://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<groupId>com.example</groupId>
<artifactId>spring-mvc-hibernate-demo</artifactId>
<version>1.0.0</version>
<packaging>war</packaging>
<properties>
<spring.version>5.3.12.RELEASE</spring.version>
<hibernate.version>5.5.7.Final</hibernate.version>
<mysql-connector.version>8.0.27</mysql-connector.version>
<log4j.version>2.17.0</log4j.version>
<jstl.version>1.2</jstl.version>
</properties>
<dependencies>
<!-- Spring MVC -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-webmvc</artifactId>
<version>${spring.version}</version>
</dependency>
<!-- Hibernate -->
<dependency>
<groupId>org.hibernate</groupId>
<artifactId>hibernate-core</artifactId>
<version>${hibernate.version}</version>
</dependency>
<!-- MySQL Connector -->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>${mysql-connector.version}</version>
</dependency>
<!-- Log4j -->
<dependency>
<groupId>org.apache.logging.log4j</groupId>
<artifactId>log4j-core</artifactId>
<version>${log4j.version}</version>
</dependency>
<!-- JSTL -->
<dependency>
<groupId>javax.servlet</groupId>
<artifactId>jstl</artifactId>
<version>${jstl.version}</version>
</dependency>
</dependencies>
<build>
<plugins>
<!-- Maven Compiler Plugin -->
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-compiler-plugin</artifactId>
<version>3.8.1</version>
<configuration>
<source>1.8</source>
<target>1.8</target>
</configuration>
</plugin>
<!-- Maven War Plugin -->
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-war-plugin</artifactId>
<version>3.3.1</version>
<configuration>
<warSourceDirectory>src/main/webapp</warSourceDirectory>
<failOnMissingWebXml>false</failOnMissingWebXml>
</configuration>
</plugin>
</plugins>
</build>
</project>
hibernate.cfg.xml :
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC 
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd"> 
<hibernate-configuration>
<session-factory>
<property
name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
<property
name="hibernate.connection.url">jdbc:mysql://localhost:3306/users/property> 
<property name="hibernate.connection.username">root</property>
<property name="hibernate.connection.password">Vikarabad@123</property>
<property
name="hibernate.dialect">org.hibernate.dialect.MySQL8Dialect</property>
<property name="hibernate.current_session_context_class">thread</property>
<property name="hibernate.show_sql">true</property>
<property name="hibernate.format_sql">true</property>
</session-factory>
</hibernate-configuration>
dispatcher-servlet.xml :
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:mvc="http://www.springframework.org/schema/mvc"
xsi:schemaLocation="http://www.springframework.org/schema/beans 
http://www.springframework.org/schema/beans/spring-beans.xsd 
http://www.springframework.org/schema/context 
http://www.springframework.org/schema/context/spring-context.xsd 
http://www.springframework.org/schema/mvc 
http://www.springframework.org/schema/mvc/spring-mvc.xsd"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<!-- Enable component scanning for Spring MVC -->
<context:component-scan base-package="com.example.controller" />
<!-- Configure Spring MVC -->
<mvc:annotation-driven />
<!-- View resolver for JSP files -->
<bean
class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="/WEB-INF/views/" />
<property name="suffix" value=".jsp" />
</bean>
</beans>
web.xml :
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://java.sun.com/xml/ns/javaee"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
id="WebApp_ID" version="3.0">
<display-name>Spring MVC Hibernate Demo</display-name>
<servlet>
<servlet-name>dispatcher</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servletclass>
<init-param>
<param-name>contextConfigLocation</param-name>
<param-value>/WEB-INF/dispatcher-servlet.xml</param-value>
</init-param>
<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
<servlet-name>dispatcher</servlet-name>
<url-pattern>/</url-pattern>
</servlet-mapping>
</web-app>
log4j2.xml :
<Configuration status="WARN">
<Appenders>
<Console name="ConsoleAppender" target="SYSTEM_OUT">
<PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} -
%msg%n" />
</Console>
</Appenders>
<Loggers>
<Root level="debug">
<AppenderRef ref="ConsoleAppender" />
</Root>
</Loggers>
</Configuration>


# practice project : 2

FeedbackController.java :
package com.controller;
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.stereotype.Controller; 
import org.springframework.web.bind.annotation.GetMapping; 
import com.service.AppService; 
@Controller 
public class FeedbackController { 
@Autowired 
private AppServiceappService; 
@GetMapping("/feedback") 
public String feedback() { 
return "feedback"; 
} 
} 
MyRestApp.java
package com.controller; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.web.bind.annotation.PostMapping; 
import org.springframework.web.bind.annotation.RequestParam; 
import org.springframework.web.bind.annotation.RestController; 
import com.entity.Feedback; 
import com.service.AppService; 
@RestController
public class MyRestApp { 
@Autowired
private AppService service; 
@PostMapping("/feedback") 
public String userRegister(@RequestParam("firstname") String firstname, 
@RequestParam("lastname") String lastname, @RequestParam("email") String 
email , 
@RequestParam("feedback1") String feedback1) { 
Feedback f = new Feedback(firstname, lastname, email,feedback1); 
boolean data= service.addFeedback(f); 
if(data) { 
return "Feedback added succesfully!"; 
} 
else { 
return "Unable to add the feedback";
}
}
}
AppService.java :

package com.service; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.stereotype.Service; 
import com.dao.MyRepo; 
import com.entity.Feedback; 
@Service
public class AppService { 
@Autowired 
private MyRepomyRepo;
public booleanaddFeedback( Feedback f) { 
myRepo.save(f); 
return true; 
} 
} 
Feedback.java
package com.entity; 
import javax.persistence.Entity; 
import javax.persistence.GeneratedValue; 
import javax.persistence.Id; 
import javax.persistence.Table; 
@Entity @Table(name="feedback") 
public class Feedback { 
@Override
public String toString() { 
return "Feedback [id=" + id + ", firstname=" + firstname + ", lastname=" + 
lastname + ", email=" + 
email+ ", feedback1=" + feedback1 + "]"; 
} 
@Id @GeneratedValue 
private int id; 
private String firstname; 
private String lastname; 
private String email; 
private String feedback1; 
public Feedback() { 
super(); 
// TODO Auto-generated constructor stub 
} 
public Feedback(String firstname, String lastname, String email ,String 
feedback1) { 
super(); 
this.firstname = firstname; 
this.lastname = lastname; 
this.email= email; 
this.feedback1 = feedback1; 
} 
} 
SpringcoreApplication.java :

package com.example.springcore; 
import org.springframework.boot.SpringApplication; 
import org.springframework.boot.autoconfigure.SpringBootApplication; 
import org.springframework.boot.autoconfigure.domain.EntityScan; 
import org.springframework.context.annotation.ComponentScan; 
import
org.springframework.data.jpa.repository.config.EnableJpaRepositories; 
import org.springframework.web.bind.annotation.GetMapping; 
@SpringBootApplication
@ComponentScan(basePackages="com") 
@EntityScan(basePackages="com") 
@EnableJpaRepositories(basePackages="com") 
public class SpringcoreApplication { 
public static void main(String[] args) { 
// TODO Auto-generated method stub

SpringApplication.run(SpringcoreApplication.class, args); 
} 
} 
FeedbackApplicationTests.java
package com.project.Feedback; 
import static org.junit.jupiter.api.Assertions.assertEquals; 
import javax.persistence.EntityManager; 
import org.junit.jupiter.api.Test; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.boot.test.context.SpringBootTest; 
import com.project.Feedback.entities.Feedback; 
import com.project.Feedback.repositories.FeedbackRepository; 
@SpringBootTest 
class FeedbackApplicationTests { 
@Autowired 
EntityManagerentityManager; 
@Autowired 
FeedbackRepositoryfeedbackRepo; 
// @Autowired 
@Test 
void shouldFindByUser() { 
Feedback testFeedback = new Feedback("Dummy Test", 5, "dummy"); 
entityManager.persist(testFeedback); 
entityManager.flush(); 
Feedback cmp = feedbackRepo.findByUser(testFeedback.getUser()); 
assertEquals(cmp.getUser(), testFeedback.getUser()); 
} 
} 
FeedbackController.java :
package com.project.Feedback.controllers; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.http.MediaType; 
import org.springframework.web.bind.annotation.GetMapping; 
import org.springframework.web.bind.annotation.PostMapping; 
import org.springframework.web.bind.annotation.RequestBody; 
import org.springframework.web.bind.annotation.ResponseBody; 
import org.springframework.web.bind.annotation.RestController; 
import com.project.Feedback.entities.Feedback; 
import com.project.Feedback.services.FeedbackService; 
@RestController 
public class FeedbackController { 
@Autowired 
FeedbackServicefeedbackService; 
@GetMapping("/feedback") 
public Iterable<Feedback>getAllFeedbacks(){ 
return feedbackService.GetAllFeedback();
} 
@PostMapping(path="/feedback", consumes= 
{MediaType.APPLICATION_JSON_VALUE}) 
public Feedback addNewFeedback(@RequestBody Feedback fb) { 
Feedback newFb = new Feedback(fb.getComments(), fb.getRating(), 
fb.getUser()); 
feedbackService.addNewFeedback(newFb); 
return newFb; 
} 
} 
TestFormController.java :
package com.project.Feedback.controllers; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.stereotype.Controller; 
import org.springframework.ui.ModelMap; 
import org.springframework.web.bind.annotation.GetMapping; 
import org.springframework.web.bind.annotation.ModelAttribute; 
import org.springframework.web.bind.annotation.PostMapping; 
import com.project.Feedback.entities.Feedback; 
import com.project.Feedback.services.FeedbackService; 
@Controller
public class TestFormController { 
@Autowired 
FeedbackServicefeedbackService; 
@GetMapping("/test_form") 
public String showTestForm(ModelMap model) { 
model.addAttribute("test", new Feedback()); 
return "testformjsp"; 
} 
@PostMapping("/test_form") 
public String submitTestForm(@ModelAttribute("testUser") Feedback fb, 
ModelMap m) { 
feedbackService.addNewFeedback(fb); 
m.addAttribute("test", fb); 
return "post"; 
}
// TODO: Implement form submission 
// TODO: call RestTemplate and make json request to localhost.../feedback 
} 
//RestTemplaterestTemplate = new RestTemplate(); 
//URL testForm = new URL("http://localhost:8090/feedback/{feedback}"); 
//ResponseEntity<String> response = restTemplate.getForEntity(testForm + 
"/7", String.class); 
//ObjectMapper mapper = new ObjectMapper(); 
//JsonNode root = mapper.readTree(response.getBody()); 
//JsonNode name = root.path("name"); 
//model.addAttribute(name); 
//String result = 
restTemplate.getForObject("http://localhost:8090/feedback/{feedback}",
String.class, 7); 
package com.project.Feedback.entities; 
Feedback.java :
import javax.persistence.Column; 
import javax.persistence.Entity; 
import javax.persistence.GeneratedValue; 
import javax.persistence.GenerationType; 
import javax.persistence.Id;
import javax.validation.constraints.NotNull; 
import lombok.Data; 
@Entity 
@Data 
public class Feedback { 
@Id 
@GeneratedValue(strategy = GenerationType.AUTO) 
@Column(name="id") 
@NotNull 
private Integer id; 
@Column(name="comments") 
private String comments; 
@Column(name="rating") 
@NotNull 
private int rating; 
@Column(name="user") 
private String user; 
public Feedback() { 
super(); 
} 
public Feedback(String comments, Integer rating, String user) { 
this.comments = comments; 
this.rating = rating; 
this.user = user; 
} 
/* 
* Needed the setters and getters to be able to add name and comments 
otherwise 
* they are nulls when entering the SQL DB 
*/
public String getComments() { 
return comments; 
} 
public void setComments(String comments) { 
this.comments = comments; 
} 
public Integer getRating() { 
return rating; 
} 
public void setRating(Integer rating) { 
this.rating = rating; 
} 
public String getUser() { 
return user; 
} 
public void setUser(String user) { 
this.user = user; 
} 
@Override 
public String toString() { 
return "Feedback [id=" + id + ", comments=" + comments + ", rating=" + 
rating + ", 
user=" + user + "]"; 
} 
} 
FeedbackRepository.java
package com.project.Feedback.repositories; 
import org.springframework.data.repository.CrudRepository; 
import org.springframework.stereotype.Repository; 
import com.project.Feedback.entities.Feedback; 
@Repository
public interface FeedbackRepository extends CrudRepository<Feedback, 
Integer> { 
public Feedback findByUser(String feedback);

}FeedbackService.java :
package com.project.Feedback.services; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.stereotype.Service; 
import com.project.Feedback.entities.Feedback; 
import com.project.Feedback.repositories.FeedbackRepository; 
@Service
public class FeedbackService { 
@Autowired 
FeedbackRepositoryfeedbackRepo; 
public Iterable<Feedback>GetAllFeedback() { 
return feedbackRepo.findAll(); 
} 
public Feedback addNewFeedback(Feedback fb) { 
return feedbackRepo.save(fb); 
} 
}
Index.jsp :
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" 
pageEncoding="ISO-8859-1"%>
<!DOCTYPE html> 
<html> 
<head> 
<meta charset="ISO-8859-1"> 
<title>Welcome Page</title> 
</head> 
<h2>Landing Page</h2> 
<body> 
<a href="test_form">Test Form</a><br/><br/> 
<a href="feedback">See all Feedbacks</a><br/><br/> 
</body> 
</html> 
Post.jsp :
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" 
pageEncoding="ISO-8859-1"%>
<!DOCTYPE html> 
<html> 
<head> 
<meta charset="ISO-8859-1"> 
<title>Post test</title> 
</head> 
<body> 
Successfully added: ${testUser.toString()} 
</body> 
</html> 
Testform.jsp :
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" 
pageEncoding="ISO-8859-1"%>
<!DOCTYPE html> 
<html> 
<head> 
<meta charset="ISO-8859-1"> 
<title>Spring test App</title> 
</head> 
<body> 
<form:form action="/test_form" method="post" commandName="testUser"> 
<label for="user">User:</label><br>


# practice project : 3

AuthenticationApplication.java
package com.example.Authentication;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Import;
import com.example.Authentication.controllers.LoginController;
import com.example.Authentication.controllers.UserDNEController;
import com.example.Authentication.entities.User;
import com.example.Authentication.exceptions.UserNotFoundException;
import com.example.Authentication.services.UserService;
@SpringBootApplication
@Import({
 UserNotFoundException.class,
 UserService.class,
 LoginController.class,
 User.class,
 UserDNEController.class
})
public class AuthenticationApplication {
public static void main(String[] args) {
SpringApplication.run(AuthenticationApplication.class, args);
}
}
userDNEController.java :
package com.example.Authentication.controllers;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import com.example.Authentication.entities.User;
import com.example.Authentication.exceptions.UserNotFoundException;
import com.example.Authentication.services.UserService;
@ControllerAdvice
public class UserDNEController {
Logger log = LoggerFactory.getLogger(LoginController.class);
@ExceptionHandler(value = UserNotFoundException.class) 
 public String errorLogin(UserNotFoundException dne){
 log.info("error found");
 return "deniedAccess";
 
 }
}
User.java :
package com.example.Authentication.entities;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
@Entity
@Table(name = "users")
public class User {
 @Id
 @GeneratedValue(strategy=GenerationType.IDENTITY)
 private Integer id;
 private String name;
 private String email;
 private String password;
 
 public User(String name, String email, String password) {
 this.name = name;
 this.email = email;
 this.password = password;
 }
 
 public User() {
 
 }
 
 public String getPassword() {
 return password;
 }
 public void setPassword(String password) {
 this.password = password;
 }
 public String getName() {
 return name;
 }
 public void setName(String name) {
 this.name = name;
 }
 public String getEmail() {
 return email;
 }
 public void setEmail(String email) {
 this.email = email;
 }
 
 @Override
 public String toString() {
 return (id.toString() + " " + name + " " + email + " " + password);
 }
 
}
UserNotFoundException.java :
package com.example.Authentication.exceptions;
public class UserNotFoundException extends RuntimeException {
 private static final long serialVersionUID = 1L;
 
 
 
}
userRepository.java :
package com.example.Authentication.repositories;
import java.util.Optional;
import org.springframework.data.repository.CrudRepository;
import com.example.Authentication.entities.User;
public interface UserRepository extends CrudRepository<User, Integer> {
 public Optional<User> findByName(String name);
}
userService.java :
package com.example.Authentication.services;
import java.util.List;
import java.util.Optional;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.example.Authentication.entities.User;
import com.example.Authentication.exceptions.UserNotFoundException;
import com.example.Authentication.repositories.UserRepository;
@Service
public class UserService {
@Autowired
private UserRepository userRepository;
 public Iterable<User> GetAllUsers()
 {
 return userRepository.findAll();
 }
 public User GetUserByName(String name) {
 Optional<User> foundUser = userRepository.findByName(name);
 if (!foundUser.isPresent()) {
 throw new UserNotFoundException();
 }
 
 return(foundUser.get());
 
 }
 
 public boolean verifyPassword(String username, String password) {
 boolean verified = false;
 User user = GetUserByName(username);
 
 if (user.getPassword().equals(password)) {
 verified = true;
 }
 
 return verified;
 }
 }
AuthenticationWebTests.java :
package com.example.Authentication
import com.example.Authentication.controllers.LoginController;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.rsocket.server.LocalRSocketServerPort;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.hamcrest.Matchers.containsString;
import static
org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static
org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static
org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static
org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
public class AuthenticationWebTests {
 @LocalRSocketServerPort
 private int port;
 @Autowired
 private LoginController controller;
 @Autowired
 private MockMvc mockMvc;
 @Test
 public void shouldReturnDefaultMessage() throws Exception {
 this.mockMvc.perform(get("/")).andDo(print()).andExpect(status().isOk());
 }
}
UserEntityTests.java :
package com.example.Authentication;
import com.example.Authentication.entities.User;
import com.example.Authentication.repositories.UserRepository;
import com.example.Authentication.services.UserService;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.assertEquals;
public class UserEntityTests {
@Test
public void WhenSetPassword_CheckGetPassword() {
User testUser = new User();
testUser.setPassword("Samplepassword");
assertEquals(testUser.getPassword(),"Samplepassword");
}
@Test
public void WhenSetPassword_CheckPassword() {
User testUser = new User();
testUser.setPassword("password");
String test = testUser.getPassword();
assertEquals(test, "password");
}
@Test
public void WhenSetName_CheckGetName() {
User testUser = new User();
testUser.setName("Samplename");
assertEquals(testUser.getName(),"Samplename");
}
@Test
public void WhenSetName_CheckName() {
User testUser = new User();
testUser.setName("name");
assertEquals(testUser.getName(),"name");
}
@Test
public void WhenSetEmail_CheckEmail() {
User testUser = new User();
testUser.setEmail("sample@test.com");
String test = testUser.getEmail();
assertEquals(test, "sample@test.com");
}
@Test
public void WhenSetEmail_CheckGetEmail() {
User testUser = new User();
testUser.setEmail("sample@test.com");
String test = testUser.getEmail();
assertEquals(test, "sample@test.com");
}
}
UserRepoTests.java :
package com.example.Authentication;
import com.example.Authentication.entities.User;
import com.example.Authentication.repositories.UserRepository;
import com.example.Authentication.services.UserService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager;
import org.springframework.boot.test.context.SpringBootTest;
import org.junit.jupiter.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.assertEquals;
import java.util.Optional;
@DataJpaTest
public class UserRepoTests {
 @Autowired
 private TestEntityManager entityManager;
 @Autowired
 private UserRepository userRepository;
 @Test
 public void whenFindByName_thenReturnUser() {
 
 User dummyUser = new User();
 dummyUser.setName("Dummy");
 dummyUser.setEmail("test@test.com");
 dummyUser.setPassword("password");
 entityManager.persist(dummyUser);
 entityManager.flush();
 
 Optional<User> found = userRepository.findByName(dummyUser.getName());
 User founded = found.get();
 assertEquals(founded.getName(), dummyUser.getName());
 }
}
UserServiceTest.java :
package com.example.Authentication;
import static org.junit.jupiter.api.Assertions.assertEquals;
import java.util.List;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager;
import com.example.Authentication.entities.User;
import com.example.Authentication.services.UserService;
@DataJpaTest
public class UserServiceTest {
@Autowired
private TestEntityManager eM;
@Autowired
private UserService us;
@BeforeEach
public void bulid() {
 eM.persist(new User("Dummy", "test@test.com", "password"));
 
 eM.persist(new User("Dummy2", "test2@test.com", "password2"));
 
 eM.flush();
}
@Test
public void testGetAllUsers() {
 
 Iterable<User> users = us.GetAllUsers();
 int count = 0;
 for (User user : users) {
 count++; 
 }
 
 assertEquals(count, 2);
}
public void testGetUserByName() {
String name = "Dummy";
User u = us.GetUserByName(name);
assertEquals(u.getName(), name);
}
@Test
public void testVerifyPassword() {
String username = "Dummy";
String password = "password";
boolean b = us.verifyPassword(username, password);
assertEquals(b, true);
}
}
Pom.xml :
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
https://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>3.1.2</version>
<relativePath/> <!-- lookup parent from repository -->
</parent>
<groupId>Mphasis</groupId>
<artifactId>Authentication</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>Authentication</name>
<description>Demo project for Spring Boot</description>
<properties>
<java.version>17</java.version>
</properties>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-devtools</artifactId>
<scope>runtime</scope>
<optional>true</optional>
</dependency>
<dependency>
<groupId>com.h2database</groupId>
<artifactId>h2</artifactId>
<scope>runtime</scope>
</dependency>
<dependency>
<groupId>com.mysql</groupId>
<artifactId>mysql-connector-j</artifactId>
<scope>runtime</scope>
</dependency>
<dependency>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
<optional>true</optional>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-test</artifactId>
<scope>test</scope>
</dependency>
<dependency>
 <groupId>org.junit.jupiter</groupId>
 <artifactId>junit-jupiter-api</artifactId>
 <version>5.7.0</version>
 <scope>test</scope>
 </dependency>
 
 <dependency>
 <groupId>org.junit.jupiter</groupId>
 <artifactId>junit-jupiter-engine</artifactId>
 <version>5.7.0</version>
 <scope>test</scope>
 </dependency>
 <dependency>
<groupId>org.apache.tomcat.embed</groupId>
<artifactId>tomcat-embed-jasper</artifactId>
<scope>provided</scope>
</dependency>
<dependency>
<groupId>javax.servlet</groupId>
<artifactId>jstl</artifactId>
<version>1.2</version>
</dependency>
</dependencies>
<build>
<plugins>
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
<configuration>
<excludes>
<exclude>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
</exclude>
</excludes>
</configuration>
</plugin>
</plugins>
</build>
</project>
Applications.properties :
Applications.properties
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://${MYSQL_HOST:localhost}:3306/StudentDetail
spring.datasource.username=root
spring.datasource.password=Ankush@112531
logging.level.org.springframework.web: DEBUG
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
server.port=8080
server.error.whitelabel.enabled=false
src/main/webapp/WEB-INF/jsp:
deniedAcess.jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
 pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Login Denied</title>
</head>
<body>
<h2 style="text-align: center">Denied Page</h2>
<p> The username and/or password was incorrect.</p>
<a href="loginform">Return to Log In</a>
</body>
</html>
Index.jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
 pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Home Page</title>
</head>
<body>
<h2 style="text-align: center">Welcome</h2>
<a href="loginform">Login</a>
</body>
</html>
Loginform.jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
 pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>User Login</title>
</head>
<body>
<h2 style="text-align: center">Login Page</h2>
<div style="color:red;">
${error} 
</div>
<form action="loginform" method='post'>
<label for="username">Name:</label><br> <input type="text"
id="username" placeholder="Name Required" name="username"
required><br>
<br> <label for="password">Password:</label><br> <input
type="password" id="password" placeholder="Password Required"
name="password" required><br>
<br> <input type="submit" value="Submit POST Request">
</form>
</body>
</html>
Success.jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
 pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Login Successful</title>
</head>
<body>
<form style="text-align: center; margin-left: auto; margin-right: auto"
action="loginform" method="get">
<label for="username"></label> <input type="hidden" name="username">
<br> <br> <label for="password"></label> <input
type="hidden" name="password"> <br> <br>
</form>
<h2 style="text-align:left">Successfully Login</h2>
<a href="loginform">Login</a>
<br>
</body>
</html>

# practice project : 4

Source Code for Implement Spring Security with Authentication

Source Code for Implement Spring Security with Authentication:
Pom:xml:
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
https://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.4.3</version>
<relativePath/> <!-- lookup parent from repository -->
</parent>
<groupId>com.example</groupId>
<artifactId>SpringSecurityManager</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>UserManager</name>
<description>Demo project for Spring Boot</description>
<properties>
<java.version>17</java.version>
</properties>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<scope>runtime</scope>
</dependency>
<dependency>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
<optional>true</optional>
</dependency>
<dependency> 
<groupId>javax.servlet</groupId> 
<artifactId>jstl</artifactId>
<version>1.2</version>
</dependency>
 <dependency>
 <groupId>org.apache.tomcat.embed</groupId>
 <artifactId>tomcat-embed-jasper</artifactId>
 <scope>provided</scope>
 </dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-test</artifactId>
<scope>test</scope>
</dependency>
</dependencies>
<build>
<plugins>
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
<configuration>
<excludes>
<exclude>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
</exclude>
</excludes>
</configuration>
</plugin>
</plugins>
</build>
</project>
User.java:
package com.example.SpringSecurityManager.entities;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
@Entity // This tells Hibernate to make a table out of this class
public class User {
 @Id
 @GeneratedValue(strategy=GenerationType.AUTO)
 private Integer id;
 private String name;
 private String email;
 private String password;
 public String getPassword() {
 return password;
 }
 public void setPassword(String password) {
 this.password = password;
 }
 public Integer getId() {
 return id;
 }
 public void setId(Integer id) {
 this.id = id;
 }
 public String getName() {
 return name;
 }
 public void setName(String name) {
 this.name = name;
 }
 public String getEmail() {
 return email;
 }
 public void setEmail(String email) {
 this.email = email;
 }
 
 @Override
 public String toString() {
 return (id.toString() + " " + name + " " + email + " " + password);
 }
 
}
User Controller.java:
package com.example.SpringSecurityManager.controllers;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.GetMapping;
import com.example.SpringSecurityManager.entities.User;
import com.example.SpringSecurityManager.services.UserService;
@Controller
public class UserController {
@Autowired
private UserService userService;
 Logger logger = LoggerFactory.getLogger(UserController.class);
@GetMapping("/users")
public String showUsers(ModelMap model) {
logger.info("Getting all Users");
Iterable<User> users = userService.GetAllUsers();
logger.info("Passing users to view");
 model.addAttribute("users", users); 
 return "users";
 }
}
Main Controller.java:
package com.example.SpringSecurityManager.controllers;
import java.util.ArrayList;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import com.example.SpringSecurityManager.entities.User;
import com.example.SpringSecurityManager.services.UserService;
@Controller
public class MainController {
@Autowired
private UserService userService;
 Logger logger = LoggerFactory.getLogger(MainController.class);
 String currID = null;
 
@GetMapping(value="/")
 public String showHomePage(ModelMap model, 
 @RequestParam(value="name", required=false, 
defaultValue="World") String name){
 model.addAttribute("name", name); 
return "home";
 }
@PostMapping(value="/index")
public String showIndexPage(@RequestParam("namelogin") String 
namelogin, @RequestParam("passwordlogin") String passwordlogin, ModelMap 
modelMap)
{
try {
User u = userService.GetUserByName(namelogin);
if(u.getName().equals(namelogin) && 
u.getPassword().equals(passwordlogin))
 {
 return "index";
}
else
{
return "home";
}
}
catch(NullPointerException e) {
return "home";
}
}
public boolean isNumber(String s)
{
if(s == null)
return false;
try
{
double db = Double.parseDouble(s);
}
catch(NumberFormatException e)
{
return false;
}
return true;
}
@PostMapping("/update") 
public String saveDetails(@RequestParam("id") String id, ModelMap 
modelMap) {
 
try
{
User user = userService.GetUserById(Integer.valueOf(id));
ArrayList<User> userList = new ArrayList<>();
if(user != null)
{
userList.add(user);
Iterable<User> users = userList;
currID = id;
modelMap.put("user", users);
}
else
return "nouser";
} 
catch (NumberFormatException e) 
{
// TODO Auto-generated catch block
return "nouser";
} 
catch (Exception e) 
{
// TODO Auto-generated catch block
e.printStackTrace();
}
modelMap.put("ID", id);
 return "update"; 
}
@PostMapping("/update2") 
public String updateDetails(@RequestParam("nameedit") String 
nameedit, @RequestParam("emailedit") String emailedit, 
@RequestParam("passwordedit") String passwordedit, ModelMap modelMap) {
ArrayList<User> userList = new ArrayList<>();
try
{
User u = 
userService.GetUserById(Integer.valueOf(currID));
userService.setUser(u, nameedit, emailedit, 
passwordedit);
userList.add(u);
Iterable<User> users = userList;
modelMap.put("user", users);
}
catch (NumberFormatException e)
{
e.printStackTrace();
}
catch(Exception e)
{
e.printStackTrace();
}
modelMap.put("IDedit", currID);
return "update2";
}
}
User Exception Controller.java:
package com.example.SpringSecurityManager.controllers;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import com.example.SpringSecurityManager.exceptions.UserNotFoundException;
@ControllerAdvice
public class UserExceptionController {
@ExceptionHandler(value = UserNotFoundException.class)
 public ResponseEntity<Object> exception(UserNotFoundException 
exception) {
 return new ResponseEntity<>("User not found", HttpStatus.NOT_FOUND);
 }
}
App error Controller.java:
package com.example.SpringSecurityManager.controllers;
import org.springframework.boot.web.servlet.error.ErrorController;
import org.springframework.web.bind.annotation.RequestMapping;
public class AppErrorController implements ErrorController {
@RequestMapping("/error")
 public String handleError() {
 //do something like logging
 return "error";
 }
 @Override
 public String getErrorPath() {
 return null;
 }
}
User Manager Application.java:
package com.example.SpringSecurityManager;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class UserManagerApplication {
public static void main(String[] args) {
SpringApplication.run(UserManagerApplication.class, args);
}
}
User Repository.java:
package com.example.SpringSecurityManager.repositories;
import org.springframework.data.repository.CrudRepository;
import com.example.SpringSecurityManager.entities.User;
public interface UserRepository extends CrudRepository<User, Integer> {
 public User findByName(String name);
}
User Service.java:
package com.example.SpringSecurityManager.services;
import java.util.Optional;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.example.SpringSecurityManager.entities.User;
import com.example.SpringSecurityManager.repositories.UserRepository;
@Service
public class UserService {
@Autowired
private UserRepository userRepository;
 public Iterable<User> GetAllUsers()
 {
 return userRepository.findAll();
 }
 public User GetUserByName(String name) {
 User foundUser = userRepository.findByName(name);
 return foundUser;
 }
 
 public User GetUserById(int id) throws Exception {
 Optional<User> foundUser = userRepository.findById(id);
 
 //TODO: we need to decide how to handle a "Not Found" condition
 if(!foundUser.isPresent())
 return null;
 
 return(foundUser.get());
 }
 
 public void UpdateUser(User usertoUpdate) {
 userRepository.save(usertoUpdate);
 }
 
 public void setUser(User u, String name, String email, String password) 
{
 //u.setId(id);
 u.setName(name);
 u.setEmail(email);
 u.setPassword(password);
 UpdateUser(u);
 }
}
UserNotFoundException.java:
package com.example.SpringSecurityManager.exceptions;
public class UserNotFoundException extends RuntimeException {
private static final long serialVersionUID = 1L;
}
Index.jsp:
<html>
<head>
<style>
.center {
 text-align: center;
 }
 
</style>
</head>
<body>
<div class="d-flex justify-content-center">
<div class="w-75 p-3">
<div class="center">
<h1 class="display-4">Search for a User By ID</h1>
<div class="jumbotron">
<h2 class="hello-title">Login Success</h2>
<p class="lead">View user table <a
href="/users">here</a></p>
<br><br>
<form method="post" action="update">
<p class="lead">Enter an id from the table: 
<p><input type="text" id="id" name="id" placeholder="Type here"
required><input type="submit" value="Enter" class="btn btn-primary mb-2"/>
</form>
</div>
</div>
</div>
</div>
</body>
</html>
Home.jsp:
<html>
<head>
<style>
.center {
 text-align: center;
 }
 
</style>
</head>
<body>
<div class="d-flex justify-content-center">
<div class="w-75 p-3">
<div class="center">
<h1 class="display-4">Spring Security</h1>
<div class="jumbotron">
<p class="lead">Login below to access the 
user's table</p>
<form method="post" action="index">
<input type="text" id="namelogin"
name="namelogin" placeholder="Name" required>
<input type="text" id="passwordlogin"
name="passwordlogin" placeholder="Password" required>
<input type="submit" value="Enter"
class="btn btn-primary mb-2" />
</form>
</div>
</div>
</div>
</div>
</body>
</html>
User.jsp:
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
<style>
table, th, td {
 border: 1px solid black;
 margin: auto;
 
}
.center {
 text-align: center;
 }
</style>
</head>
<body>
<div class="d-flex justify-content-center">
<div class="w-75 p-3">
<div class="center">
<div class="jumbotron">
<h2 class="display-4">Users</h2>
<table style="float:inherit">
 
<tr><th>ID</th><th>Name</th><th>Email</th><th>Password</th></tr>
 <c:forEach items="${users}" var="user"
varStatus="count">
 <tr id="${count.index}">
 <td>${user.id}</td>
 <td>${user.name}</td>
 <td>${user.email}</td>
 <td>${user.password}</td>
 </tr>
 </c:forEach>
</table>
</div>
 </div>
</div>
</div>
</body>
</html>
Update.jsp:
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
<style>
table, th, td {
 border: 1px solid black;
 margin: auto;
 
}
.center {
text-align: center;
}
</style>
</head>
<body>
<div class="d-flex justify-content-center">
<div class="w-75 p-3">
<div class="center">
<div class="jumbotron">
<h2 class="display-4">Update Table</h2>
<p class="lead"> User ID: ${ID}</p>
<table style="float:inherit">
 
<tr><th>ID</th><th>Name</th><th>Email</th><th>Password</th></tr>
 <c:forEach items="${user}" var="userE"
varStatus="count">
 <tr id="${count.index}">
 <td>${userE.id}</td>
 <td>${userE.name}</td>
 <td>${userE.email}</td>
 <td>${userE.password}</td>
 </tr>
</c:forEach>
</table>
<br><br>
<form method="post" action="update2">
<br><h3>Edit user: ${ID}</h3>
<input type="text" id="nameedit"
name="nameedit" placeholder="Name" required>
<input type="text" id="emailedit"
name="emailedit" placeholder="Email" required>
<input type="text" id="passwordedit"
name="passwordedit" placeholder="Password" required>
<input type="submit" value="Enter" class="btn 
btn-primary mb-2"/>
</form>
</div>
</div>
</div>
</div>
</body>
</html>
Update2.jsp:
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
<style>
table, th, td {
 border: 1px solid black;
 margin: auto;
}
.center {
 text-align: center;
 }
</style>
</head>
<body>
<div class="d-flex justify-content-center">
<div class="w-75 p-3">
<div class="center">
<h2 class="display-4">Successfully Updated User</h2>
<div class="jumbotron">
<p class="lead"> User ID: ${IDedit}</p>
<div>
<table style="float:inherit">
 
<tr><th>ID</th><th>Name</th><th>Email</th><th>Password</th></tr>
 <c:forEach items="${user}" var="userE"
varStatus="count">
 <tr id="${count.index}">
 <td>${userE.id}</td>
 <td>${userE.name}</td>
 <td>${userE.email}</td>
 <td>${userE.password}</td>
 </tr>
</c:forEach>
</table>
</div>
<br><br>
<h3>Return to Homepage</h3>
<div>
<a href="/">Return</a>
</div>
</div>
</div>
</div>
</div>
</body>
</html>
Error.jsp:
<html>
<head>
</head>
<body>
<h2>Error: Page not found</h2>
</body>
</html>
No user.jsp:
<html>
<head>
</head>
<body>
<h2>Error: User not found</h2>
</body>
</html>
Application.properties:
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/springboot
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=root
logging.level.org.springframework.web: DEBUG
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
