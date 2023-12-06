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




# practice project2 [VALIDATION OF EMAIL]


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


# practice project :3 [FILE HANDELLING]

package Practice;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileOperation {

    public static void main(String[] args) {
        // Specify the file path
        String filePath = "C:\\Users\\Dell\\Desktop\\AssistedPractice\\newfile";

        // Write to a file
        writeToFile(filePath, "Hello, this is a file handling example!");

        // Read from the file
        String content = readFromFile(filePath);
        System.out.println("Content read from the file:\n" + content);
    }

    // Method to write content to a file
    private static void writeToFile(String filePath, String content) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            writer.write(content);
            System.out.println("Content successfully written to the file.");
        } catch (IOException e) {
            System.err.println("Error writing to the file: " + e.getMessage());
        }
    }

    // Method to read content from a file
    private static String readFromFile(String filePath) {
        StringBuilder content = new StringBuilder();
        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = reader.readLine()) != null) {
                content.append(line).append("\n");
            }
        } catch (IOException e) {
            System.err.println("Error reading from the file: " + e.getMessage());
        }
        return content.toString();
    }
}


# practice project 4: [longest increasing subsequence]


package Practice;

import java.util.Arrays;

public class LongestIncreasingSubsequence {

    public static int longestIncreasingSubsequence(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }

        int[] lis = new int[n];
        Arrays.fill(lis, 1);

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j] && lis[i] < lis[j] + 1) {
                    lis[i] = lis[j] + 1;
                }
            }
        }

        int maxLength = Arrays.stream(lis).max().orElse(1);
        return maxLength;
    }

    public static void main(String[] args) {
        int[] nums = {15, 22, 9, 33, 23, 50, 31, 65, 83};
        int result = longestIncreasingSubsequence(nums);
        System.out.println("Length of Longest Increasing Subsequence: " + result);
    }
}


# practice project 5[Fix Bugs]


package Practice;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;
public class FixBugs {
public static void main(String[] args) {
System.out.println("\n*************\n");
System.out.println("\tWelcome to TheDesk \n");
System.out.println("**************");
optionsSelection();
}
private static void optionsSelection() {
String[] arrMenuOptions = {"1. I wish to review my expenditure",
		                   "2. I wish to add my expenditure",
		                   "3. I wish to delete my expenditure",
		                   "4. I wish to sort the expenditures",
		                   "5. I wish to search for a particular expenditure",
		                   "6. Close the application"
		                    };
int[] arr1 = {1, 2, 3, 4, 5, 6};
ArrayList<Integer>arrlist = new ArrayList<Integer>();
ArrayList<Integer>expenses = new ArrayList<Integer>();
expenses.add(1200);
expenses.add(500);
expenses.add(3000);
expenses.add(7000);
expenses.add(220);
expenses.addAll(arrlist);
Scanner sc = new Scanner(System.in);
int options = 0;
while (options != 6) {
displayMenuOptions(arrMenuOptions);
System.out.println("\nEnter your choice:\t");
options = sc.nextInt();
switch (options) {
case 1:
		System.out.println("Your saved expenses are listed below: \n");
		if (expenses.isEmpty()) 
		{
		System.out.println("Expenses list is empty\n");
		 } 
		else 
		{
		System.out.println(expenses + "\n");
		 }
		break;
case 2:
		System.out.println("Enter the value to add your Expense: \n");
		int value = sc.nextInt();
		expenses.add(value);
		System.out.println("Your value is updated\n");
		expenses.addAll(arrlist);
		System.out.println(expenses + "\n");
		break;
case 3:
		System.out.println("You are about the delete all your expenses! \nConfirm again by selecting the same option...\n");
		int con_choice = sc.nextInt();
		if (con_choice == options)
		{
		expenses.clear();
		System.out.println(expenses + "\n");
		System.out.println("All your expenses are erased!\n");
		 }
		else
		{
		System.out.println("Oops... try again!");
		  }
		break;
case 4:	
		sortExpenses(expenses);
		break;
case 5:
		searchExpenses(expenses);
		break;
case 6:
		closeApp();
		break;
default:
		System.out.println("You have made an invalid choice!\nTry again!\n");
		break;
		            }
		        }
		    }
private static void displayMenuOptions(String[] arrMenuOptions) {
int slen = arrMenuOptions.length;
for (int i = 0; i<slen; i++)
{
		// display the all the Strings mentioned in the String array
System.out.println(arrMenuOptions[i]);
 }
    }
private static void closeApp() {
System.out.println("Closing your application... \nThank you!");
    }
private static void searchExpenses(ArrayList<Integer>arrayList) {
int leng = arrayList.size();
System.out.println("Enter the expense you need to search:\t");

//Complete the method
if (leng> 0) {
Scanner sc = new Scanner(System.in);
int searchExpenseNo = sc.nextInt();
int i = 0;
for (i = 0; i<leng; i++) {
if (arrayList.get(i) == searchExpenseNo) {
break;
                }
            }
if (i != leng) {
System.out.println("Expenditure " + searchExpenseNo + " was found on entry no " + (i + 1) + "\n");
}else {
System.out.println("Expenditure " + searchExpenseNo + " was not found\n");
}
} else {
System.out.println("There are no expenditures available\n");
        }
    }
private static void sortExpenses(ArrayList<Integer>arrayList) {
//Complete the method. The expenses should be sorted in ascending order.
if (arrayList.isEmpty()) {
System.out.println("Sort not performed,\nExpenditure list is empty\n");
        } else {
Collections.sort(arrayList);
System.out.println("Expenditure list sorted");
System.out.println(arrayList + "\n");
        }
    }
}
