### Webdriver tips on Design Pattern

- Tip 7: Let’s say you have a list of Web elements and you wish to transform the text content of each web element in upper case. 
```java
/*With Map*/

	public List<String> getTextValueinUppercase(){
		List<WebElement> myElements = driver.findElements(allElementsLoc);
		List<String> textElementsInUpperCase = myElements.stream().map((e) -> e.getText().toUpperCase()).collect(Collectors.toList());
		return textElementsInUpperCase;
	}

```
- Tip 8:  Encapsulate the test data. I often see that while creating the test data objects we pass several parameters to test data object. And it looks awful and considered to be a code smell. Hence, we should encapsulate the data using the Java composition techniques.

For example:
```java
private WebDriver driver;
	
	public void testUI() {
		//Create test data
		Employee emp = new Employee("Jack", 
"Ryan", "Technology",
 "D12", "1 Market St", "San Francisco", "CA");
		EmployeePage empPage= new EmployeePage(driver);
		empPage.createEmployee(emp);
	}
```
If you look at the above code, the author sending the data to the Employee constructor which looks tough to maintain and not easy to read. Also the author enforcing the user to specify all the parameters which the user may not be keen to provide in some cases. 

Ideally, it is not advisable to pass more than 2 parameters to a constructor or any method. 

Effective Java book by Joshua bloch recommends Composition over inheritance. So let’s rewrite this Employee test data object.  3 classes will be created for Address, Department and Employee 
```java
@BeforeClass
public void createEmpData() {
	Address address = new Address("1 Market St","San Francisco","CA");

	Department dept=  new Department("Technology","D12");
	
	MyEmployee empData =new MyEmployee("Jack", "Ryan");
		
	empData.setAddress(address);
	empData.setDept(dept);
}
```
The Employee data object looks clean and easy to understand.
```java
@Test
public void testCreateEmp() {		
	EmployeePage empPage= new EmployeePage(driver);
	empPage.createEmployee(empData);
}
```
EmployeeClass   please note Getters & Setters are elided

```java
public class Employee {

	private Address address;
	private Department dept;
	
	public MyEmployee(String firstName, String lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}

}
```
Department class   please note Getters & Setters are elided

```java
public class Department {

	private String department;
	private String deptId;
	public Department(String string, String string2) {
	}
}
```
Address class   please note Getters & Setters are elided

```java
public class Address {

	private String address;
	private String city;
	private String state;
	public Address(String string, String string2, String string3) {
		// TODO Auto-generated constructor stub
	}


