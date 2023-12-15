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
