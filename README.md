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
