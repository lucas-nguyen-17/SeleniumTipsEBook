### Page Object & Loadable Design pattern

- **Tip 19:** Writing custom By class locator.  I am going to show one of the possibilities by writing a customized By class. While going through one of the popular Selenium blogs, I learnt that you can extend a ByClass.


Custom By class.
```java
public class ByClass extends By {
	
	private String value;
	private String tag;	
	
	public ByClass(String tag, String value) {
		this.tag=tag;
		this.value=value;
	}
	@Override
	public List<WebElement> findElements(SearchContext driver) {
		String locator = String.format("//%s[@class='%s'", tag,value);
		return driver.findElements(By.xpath(locator));
		
	}
}

public void testByClass() {		
	ByClass by = new ByClass("div", "class1");
	WebDriverWait wait = new WebDriverWait(driver, 10);
	wait.until(ExpectedConditions.presenceOfElementLocated(by));
}
```
- **Tip 20:** Blend of LoadableComponent & PageObjectModel design patterns. LoadableComponent gives you a greater flexibility to validate if the page you’re looking for is properly rendered on the UI.
This component provides 2 methods load & isLoaded. These methods will be executed when you instantiate the page object class.

```java
LoginPageObject:

public class LoginPage extends LoadableComponent<LoginPage>{

	private String url="example.com";
	private String title="My Login page";
	private WebDriver driver;
	
	@FindBy(xpath="//input[@class='login']")
	public WebElement loginName;
	
	@FindBy(xpath="//input[@class='pwd']")
	public WebElement password;
	
	@FindBy(id="home")
	public WebElement homePageIcon;
	
	public LoginPage(WebDriver driver) {
		this.driver=driver;
		PageFactory.initElements(driver,this);
	}
	@Override
	protected void isLoaded() throws Error {
		if (!driver.getCurrentUrl().contains(title)) {
			throw new RuntimeException("Login page not found");
		}
	}
	
	@Override
	protected void load() {
		driver.get(url);
	}
}
```
Let’s assume the once user is logged into the system the default behavior of the app is to land the user on Dashboard page. And to navigate to Home page the application expects the user to click on home page side navigation link. In this case, we can customize the load method of Home page Object as:
```java
public class HomePage extends LoadableComponent<HomePage>{

	private String url="homepage.com";
	private String title="My Home page";
	private WebDriver driver;
	
	@FindBy(xpath="//a[@class='test']")
	public WebElement homePageIcon;
	LoadableComponent<?> parent;
	@Override
	protected void isLoaded() throws Error {
		if (!driver.getCurrentUrl().contains(title)) {
			throw new RuntimeException("Home page not found");
		}
	}

	@Override
	protected void load() {
		((LoginPage)parent).goToHomePage();
	}
	public HomePage(WebDriver driver,LoadableComponent<?> parentPage) {
		this.driver=driver;
		this.parent=parentPage;
		PageFactory.initElements(driver,this);
	}
}

Test Class:

public class TestLoadable {

	private WebDriver driver;
	public void testPages() {
		LoginPage loginPage = new LoginPage(driver);
		HomePage homePage = new HomePage(driver, loginPage);
	}
}


