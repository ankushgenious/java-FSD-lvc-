# java-FSD-lvc-

PHASE 2 
# project 1: Validation of user login

HTML file:
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
	<form action="LoginServlet" method="get">
		

		username <input type="text" name="username"><br>
		password <input type="text" name="password"><br> 
		<input type="submit" value="click here to process">

		
	</form>

</body>
</html>


Sevlet:

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginServlet
 */
@WebServlet("/LoginServlet")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public LoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		PrintWriter pw=response.getWriter();
		String username=request.getParameter("username");
		String password=request.getParameter("password");
		if(username.equals(password))
		{
			pw.print("login successfull" +username);
		}
		else {
			pw.print("check the details");
		}
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

Web.XML
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
  <display-name>Login</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
    <description></description>
    <display-name>LoginServlet</display-name>
    <servlet-name>LoginServlet</servlet-name>
    <servlet-class>LoginServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>LoginServlet</servlet-name>
    <url-pattern>/LoginServlet</url-pattern>
  </servlet-mapping>
</web-app>


# project : 2  Retrieving the Product Details Using the Product ID.
Source code:
Index.html
<!DOCTYPE html>
<html>
<head>
 <title>Product Details</title>
</head>
<body>
 <h1>Product Details</h1>
 <form action="ProductDetails" method="GET">
 Enter ProductID: <input type="text" id="product_ID"
name="product_ID">
 <button type="submit">Get Details</button>
 </form>
</body>
</html>
Product Servlet:
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
@WebServlet("/ProductServlet")
public class ProductServlet extends HttpServlet {
private static final long serialVersionUID = 1L;
 
 String DB_URL = "jdbc:mysql://localhost:3306/productddetails";
 String DB_USER = "root";
 String DB_PASSWORD = "Lokeshmudhiraj@25";
protected void doGet(HttpServletRequest request, HttpServletResponse 
response) throws ServletException, IOException {
// Get the product ID from the request parameter
 String productId = request.getParameter("product_ID"); // Updated 
parameter name
 // Initialize the database connection and prepared statement
 Connection conn = null;
 PreparedStatement stmt = null;
 // Create a PrintWriter to write the response
 response.setContentType("text/html");
 PrintWriter out = response.getWriter();
 try {
 // Load the JDBC driver
 Class.forName("com.mysql.cj.jdbc.Driver");
 // Establish the database connection
 conn = 
DriverManager.getConnection("jdbc:mysql://localhost:3306/productddetails","
root","Lokeshmudhiraj@25");
 // Prepare the SQL query to retrieve product details using the 
product ID
 String sql = "SELECT * FROM product WHERE product_ID = ?";
 stmt = conn.prepareStatement(sql);
 stmt.setString(1, productId);
 // Execute the query and retrieve the result set
 ResultSet rs = stmt.executeQuery();
 // Check if the product is found
 if (rs.next()) {
 int productID = rs.getInt("product_ID");
 String productName = rs.getString("product_Name");
 double productPrice = rs.getDouble("product_Price");
 String productDescription = 
rs.getString("product_Description");
 // Display the product details
 out.println("<h2>Product Details:</h2>");
 out.println("<p>Product ID: " + productID + "</p>");
 out.println("<p>Product Name: " + productName + "</p>");
 out.println("<p>Product Price: " + productPrice + "</p>");
 out.println("<p>Product Description: " + productDescription 
+ "</p>");
 } else {
 // Product not found, display an error message
 out.println("<h2>Error:</h2>");
 out.println("<p>Product not found.</p>");
 }
 // Close the result set, statement, and connection
 rs.close();
 stmt.close();
 conn.close();
 } catch (SQLException | ClassNotFoundException e) {
 e.printStackTrace();
 // Handle any errors that occur during the database connection 
or query execution
 out.println("<h2>Error:</h2>");
 out.println("<p>Failed to retrieve product details.</p>");
 } finally {
 out.close();
 }
 }
}


# project : 3  Adding a new product in the Database

Jsp page 
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<div align="center">
<h2>Add a New Product</h2>
<form action="Product" method="post">
Enter ID: <input type="number" id="pid"
name="pid"
placeholder="Enter id here..."
required> <br /> <br />
Enter TYPE <input type="text" id="name"
name="name"
placeholder="Enter name here..."
required> <br /> <br />
Enter MODEL: <input type="text" id="color"
name="color"
placeholder="Enter color here..."
required> <br /> <br />
Enter NAME: <input type="text" id="price"
name="price"
placeholder="Enter number here..."
required> <br /> <br /> <input
type="submit" value="Submit">
</form>
</div>


</body>
</html>

Product
public class Product {
	private int pid; 
	private String type;
	private String model;
	private String Name;
	public int getPid() {
		return pid;
	}
	public void setPid(int pid) {
		this.pid = pid;
	}
	public String getType() {
		return type;
	}
	public void setType(String type) {
		this.type = type;
	}
	public String getName() {
		return Name;
	}
	public void setName(String name) {
		Name = name;
	}
	public String getModel() {
		return model;
	}
	public void setModel(String model) {
		this.model = model;
	}

}


Add Product

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class AddProduct
 */
@WebServlet("/AddProduct")
public class AddProduct extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private static final Product Product = null;

       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public AddProduct() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		int pid =Integer.parseInt(request.getParameter("txtPid")); 
		String type = request.getParameter("txttype");
		String model = request.getParameter("txtmodel");
		String name = request.getParameter("txtname");
		Product pro=new Product();
		pro.setPid(pid);
		pro.setType(type);
		pro.setModel(model);
		pro.setName(name);
		ProductOperation bop = new ProductOperation();
		bop.AddProduct(Product);
		response.setContentType("text/jsp");
		PrintWriter out = response.getWriter();
		out.print("<h2>Product Added....</h2>");
		request.getRequestDispatcher("index.jsp").include(request, response);
		
	}

}



View Product

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ViewProduct
 */
@WebServlet("/ViewProduct")
public class ViewProduct extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private static final Product[] Productall = null;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public ViewProduct() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		ProductOperation bop = new ProductOperation();
		List<Product> productall = bop.ViewAll();
		response.setContentType("text/jsp");
		PrintWriter out = response.getWriter();
		out.print("<table width='100%' border='1'>");
		out.print("<tr><th>Book Number</th><th>Book Name</th><th>Author</th></tr>");
		for(Product p: Productall)
		{
		out.print("<tr>");
		out.print("<td>" + p.getPid() + "</td>");
		out.print("<td>" + p.getType() + "</td>");
		out.print("<td>" + p.getModel() + "</td>");
		out.print("<td>" + p.getName() + "</td>");
		out.print("</tr>");
		}
		out.print("</table>");

	}

}




Xml page
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration SYSTEM
"http://www.hibernate.org/dtd/hibernate-configuration3.0.dtd">
<hibernate-configuration>
<session-factory>
<property
name="hibernate.dialect">org.hibernate.dialect.MySQL5Diale
ct </property>
<property
name="hibernate.connection.driver_class">com.mysql.cj.jdbc
.Driver </property>
<property
name="hibernate.connection.url">jdbc:mysql://localhost:330
6/mphasis_db</property>
<property name="hibernate.connection.username">
root 
</property>
<property name="hibernate.connection.password">
suvashree#1234
</property>
<property name="show_sql">true </property>
<property name="hbm2ddl.auto">update </property>
<mapping class="com.practice.Product"/>
</session-factory>
</hibernate-configuration>

Db connection
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
public class DbConnection {
public static SessionFactory getConnection()
{
Configuration cfg = new Configuration();
cfg.configure("hibernate.cfg.xml");
SessionFactory sf = cfg.buildSessionFactory();
return sf;
}
}


# project : 4 Product Details Portal.:



Product Details Portal.:
ValidationServlet.java
package details;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
@WebServlet("/ValidationServlet.java")
public class ValidationServlet extends HttpServlet {
private static final long serialVersionUID =
1L;
protected void doGet(HttpServletRequest
request, HttpServletResponse response) throws
ServletException, IOException {
// TODO Auto-generated method stub
String pId =
request.getParameter("productId");
String pName =
request.getParameter("productName");
String pPrice =
request.getParameter("productPrice");
HttpSession theSession =
request.getSession();
theSession.setAttribute("pid", pId);
theSession.setAttribute("pname", pName);
theSession.setAttribute("pprice", pPrice);
response.sendRedirect("display.jsp");
}
}
Product.jsp:
<%@ page language="java" contentType="text/html;
charset=ISO-8859-1"
pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Product Details</title>
</head>
<body>
<h1>enter the details of the product </h1>
<hr>
<form action="ValidationServlet.java">
<input type="text" name="productId"placeholder="PRODUCT ID"><br>
<input type="text" name="productName"placeholder="PRODUCT NAME"><br>
<input type="text" name="productPrice"splaceholder="PRODUCT PRICE"><br>
<input type="submit" value="ENTER">
</form>
</body>
</html>
Display.jsp:
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
 pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<h1>Displaying the Product Details....</h1>
<hr>
<%= "Product Id : " + session.getAttribute("pid")
%> <br> <br>
<%= "Product Name : " +
session.getAttribute("pname") %> <br> <br>
<%= "Product Price : " +
session.getAttribute("pprice") %>
</body>
</html>
Web.xml:
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID"
version="4.0">
 <display-name>ProductDetailsPortal</display-name>
 <welcome-file-list>
 <welcome-file>Product.jsp</welcome-file>
 
 </welcome-file-list>
</web-app>

