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

