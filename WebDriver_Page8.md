### Dyanmic web elements

- Tip 15:  Let’s if you want to verify if every page has an expected url and title before you do an action on that page. You can do this using ExpectedConditions<Boolean> interface. You need to write a custom expected condition which has Boolean has a type parameter.

Here the Custom ExpectedCondtion class is PageLoaded

```java
public class PageLoaded implements ExpectedCondition<Boolean>{

	private String url;
	private String title;
	
	public PageLoaded(String title,String url) {
		this.url=url;
		this.title=title;
	}
	@Override
	public Boolean apply(WebDriver driver) {
		return (driver.getCurrentUrl().contains(url) && driver.getTitle().contains(title));
	}

}

EmployeePageObject Class:

public class EmployeePage {
	
	public WebElement empNameLoc;
	public WebElement loadIcon;
	private WebDriver driver;
	private String url="https://example.org";
	private String title ="Employee Details";
	
	private void waitForEmployeePageToLoad(){
		WebDriverWait wait = new WebDriverWait(driver, 20);
		if (!wait.until(new PageLoaded(title,url))) {
			throw new RuntimeException("Employee page not loaded");
		}
	}
	public void createEmployee(EmployeeData employeeData) {
		waitForEmployeePageToLoad();
		//Create employee here...
	
}


TestPageLoaded	

public class TestPageLoaded {
	
	private WebDriver driver;
	private WebDriverWait wait;
	EmployeePage empPage;
	
	@BeforeClass
	public void setUp() {
		empPage=new EmployeePage(driver);		
	}
	@Test
	public void testEmpPage() {
		empPage.createEmployee(new EmployeeData());
	}

}
```
- Tip 16:  Let’s say if you have a dynamic webelement like a progress bar or any element whose attributes will be changed based on the some logical condition in the UI.
To test these dynamic elements is a bit challenging.  The better way to solve this via ExpectedConditions interface with WebElement as type parameter.

```java
public class DynamicWebElement implements ExpectedCondition<WebElement> {
	
	private By loc;
	private String attribute;
	
	public DynamicWebElement(String attribute,By loc) {
		this.attribute=attribute;
		this.loc=loc;
	}
	
	@Override
	public WebElement apply(WebDriver driver) {
		WebElement element = driver.findElement(loc);
		return element.getAttribute(attribute)!=null? element:null;
	}

}

At runtime, the apply method checks if the element has an attribute you’re searching for.
If not, it sends null and webdriver wait retry the condition. Otherwise, it returns the element.
This ExpectedCondition applies only to a specific element.

SamplePageObject class:

	private DynamicWebElement dynamicEle;
	private String locator="//input[@class='class1']";
	
	public void waitForDynamicElementToAppear(By loc) {
		dynamicEle=new DynamicWebElement("data-html", loc);
		WebDriverWait wait = new WebDriverWait(driver, 10);
		wait.until(dynamicEle);
	}


