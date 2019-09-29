### Tips on Assertion

- Tip 9: Do not use assertions in your page object class. It is the responsibility of the test class to assert the behavior of your application.

For example: the below page class doing an assertion in isEmployeePageDisplayed() which is not correct. Instead the author should just return Boolean value.
```java
public class EmployeePage {
	
	public WebElement empNameLoc;
	public WebElement loadIcon;

	public EmployeePage(WebDriver driver) {
		// TODO Auto-generated constructor stub
	}

	//NOT RECOMMENDED
	public void isEmployeePageDisplayed() {
		assertEquals(empNameLoc.isDisplayed(),true);
		
	}

	//RECOMMENDED
public boolean isEmployeePageDisplayed() {
return empNameLoc.isDisplayed();
		
	}

	public void clickEmpLoadIcon() {
		if (isEmployeePageDisplayed()){
			loadIcon.click();
}
	
	}
}


```
- Tip 10: Always prefer to use TestNG setup & Teardown methods for creating any test data and cleanup activities.

```java
@BeforeClass
public void createEmpData() {		
	MyEmployee empData =new MyEmployee("Jack", "Ryan");		
	empData.setAddress(address);
}
@Test
public void testCreateEmp() {		
	EmployeePage empPage= new EmployeePage(driver);		empPage.createEmployee(empData);
}	
@AfterClass
public void closeBrowser() {
	if(null!=driver) {
		driver.close();
		driver.quit();
		}
}
```
