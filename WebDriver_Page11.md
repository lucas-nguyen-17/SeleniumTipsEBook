### Exception & HTML Table parsing

- **Tip 21:** The exceptions thrown by the Selenium webdriver may not help you to understand exceptions better. For Example, NoSuchElementException or StaleElementException is easy for a QE to understand but not a business stakeholder or any non-QE stakeholder. Hence it is good to write CustomExceptionClass.
```java
public class CustomException extends RuntimeException{

	private static final long serialVersionUID = 1L;

	public CustomException() {
	}
	//throw exception with custom message
	public CustomException(String message){
		super(message);
	}
	
	//Throw exception with custom message and throw the original exception  
	public CustomException(String message,Throwable t){
		super(message,t);
	}
}

TestClass
public void testCustomException() {
	try {
		LoginPage login = new LoginPage(driver);
	}
	catch(Exception ex) {
		throw new CustomException("Page not found",ex);
	}
}
```
- **Tip 22:**  How to switch between multiple windows efficiently via Window handles. Let’s assume that you have 3 windows opened by Selenium webdriver and the user wants to switch to the correct window.

@Test
public void testWindows() {
		Set<String> windowHandles = driver.getWindowHandles();
		for(String windHandid: windowHandles) {
			driver.switchTo().window(windHandid);
			if (driver.getTitle().contains("expected title") || driver.getCurrentUrl().contains("expected Url")) {
				break;
			}
		}
}

- **Tip 23:**  How to read HTML table and fetch values of a specific column. This is possible using Java 8 streams and chain of locators. Chaining locators is another technique to use if you want to read all children in a specific parent.
```java
public void readHTMLTable() {
	WebElement tableElement= driver.findElement(By.xpath("table"));
	List<WebElement> allRows = tableElement.findElements(By.tagName("tr"));
List<List<WebElement>> allColumnsOfEachRow = allRows.stream().map((e) -> 
e.findElements(By.tagName("td"))).collect(Collectors.toList());		
}
```
If you want read a specific column from HTML table and fetch all text values.
```java
public void readSpecificCol(int col) {
	WebElement tableElement= driver.findElement(By.xpath("table"));
	List<WebElement> allRows = tableElement.findElements(By.tagName("tr"));
	 List<String> collect = allRows.stream().map((e)->
e.findElements(By.tagName("td")).get(col).getText()).collect(Collectors.toList());}
```
- **Tip 24**:  It is good to create BaseTest class instead of creating driver objects in each Testclass. This way you can control the driver object from the base class

TestBase class
```java
public class TestBase {
	
	private WebDriver driver;
	
	protected LoginPage openLogin() {
		LoginPage loginPage = new LoginPage(driver);
		return loginPage;
	}
	@BeforeClass
	public void setUp() {
		driver= new FirefoxDriver();		
	}
	
	@AfterClass
	public void close() {
		if(null!=driver) {
		
			driver.quit();
		}
	}

}
```
TestClass:
```Java
public class TestLogin extends TestBase {
	
	@Test
	public void testEmployee() {
		LoginPage loginPage = openLogin();
		loginPage.goToHomePage();
	}
}

