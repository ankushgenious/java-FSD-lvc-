# java-FSD-lvc-
# project 1: create Arithmatic Calculator for Basic operations

package Practice;
import java.util.Scanner;

public class ArihmaticCalculator {
	
   public static void Add(int x, int y) {
    	
    	int sum = (x+y);
    	System.out.println ("sum of two numbers is"+sum);
    	//return(x+y);
    	
    }
    
 static void sub(int x, int y) {
    	
    	int sub = (x-y);
    	
    	System.out.println("subtraction of two number is "+sub);
    	
    }
 
 static void mul(int x, int y) {
 	
 	int mul = (x*y);
 	
 	System.out.println("multiplication of two number is "+mul);
 	
 	
 }
 
 static void div(int x, int y) {
 	
 	int div = (x/y);
 	
 	System.out.println("divison of two number is "+div);
 	
 }
    
    
	
	public static void main (String args[]) {
	
		System.out.println("enter the two numbers to perform Arithmatic calculations ");
		
		Scanner sc = new Scanner(System.in);
		int a = sc.nextInt();
		int b = sc.nextInt();
		
		Add( a,  b);
		sub( a,  b);
		mul( a,  b);
		div( a,  b);
		
		
		
		
		
	}

}




#practice project2 [VALIDATION OF EMAIL]


package Practice;

import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
public class EmailValidation {
	
   private static final String regex = "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$";
   
   public static void main(String args[]) {
	   
      
       List<String> emails = new ArrayList<String>();
       
       System.out.println("Checking email addresses which are following pattern and which one does not....\n");
       
       emails.add("ankush@example.com");
       emails.add("aman@example.co.in");
       emails.add("atul@example.me.org");
       emails.add("ashraf@example.com");
       emails.add("aseem@example.com");
       
       
       
       //These are  email addresses which are not following our declared pattern as before @ only [A-Za-z0-9] can come
       
       
       emails.add("@example.com");
       emails.add("alice&example.com");
       emails.add("alice#@example.me.org");
       
       													
       
       Pattern pattern = Pattern.compile(regex); //initializing the pattern object
       
       //searching for occurrences of regex
       for (String value : emails) {
           Matcher matcher = pattern.matcher(value);
           System.out.println("Email " + value + " is " + (matcher.matches() ? "valid" : "invalid"));
       }
   }
}

#
