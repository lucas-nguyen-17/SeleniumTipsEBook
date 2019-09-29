### Creating a custom webdriver & typekey slowly


- Tip 13: In some of custom HTML input fields provides type assist list and expects the user to select one from the assisted list. It is similar to searching for an address in Google maps.
 So to mimic this behavior the characters need to be typed either with a delay of 100 or 200ms.

```java
public void typeKeysSlowly() throws Exception {		
	String textToType ="1 Market Street";		
	for(int i=0;i<textToType.length();i++) {			
  driver.findElement(elementlocator).sendKeys(String.valueOf(textToType.charAt(i)));
	Thread.sleep(200);
	}
}
```
- Tip 14: Create a custom driver class that implements Webdriver interface and overrides its methods. This gives you greater control and allows to create a set of unique customizable web element methods.

```java
For example:
public class WebBrowserDriver implements WebDriver{
	private WebDriver driver;
	private int timeOut=10;
	WebDriverWait wait;
	public WebBrowserDriver(WebDriver driver) {
		this.driver=driver;
		wait=new WebDriverWait(driver,timeOut);
	}
	@Override
	public void close() {
		if(null!=driver) {
			driver.close();
		}
		
	}
	public boolean isElementInvisible(By loc) {
		return wait.until(ExpectedConditions.invisibilityOfElementLocated(loc));
	}
	public WebElement getVisibleElement(By loc) {
		return wait.until(ExpectedConditions.visibilityOfElementLocated(loc));
	}

	@Override
	public WebElement findElement(By locator) {
		WebElement webElement = wait.until(ExpectedConditions.presenceOfElementLocated(locator));
		return webElement;		
		
	}
	public WebElement findClickableElement(By locator) {
		WebElement webElement = wait.until(ExpectedConditions.elementToBeClickable(locator));
		return webElement;
	}
	
	public boolean urlToBe(String url) {
		return wait.until(ExpectedConditions.urlToBe(url));
	}
	@Override
	public List<WebElement> findElements(By locator) {
		
		return wait.until(ExpectedConditions.presenceOfAllElementsLocatedBy(locator));
	}

	@Override
	public void get(String url) {
		driver.get(url);
	}

	@Override
	public String getCurrentUrl() {
		return driver.getCurrentUrl();
	}

	@Override
	public String getPageSource() {
		return driver.getPageSource();
	}

	@Override
	public String getTitle() {
		return driver.getTitle();
	}

	@Override
	public String getWindowHandle() {
		return driver.getWindowHandle();
	}

	@Override
	public Set<String> getWindowHandles() {
		return driver.getWindowHandles();
	}

	@Override
	public Options manage() {
		return driver.manage();
	}

	@Override
	public Navigation navigate() {
		return driver.navigate();
	}

	@Override
	public void quit() {
		if (null!=driver) {
			driver.quit();
		}
	}

	@Override
	public TargetLocator switchTo() {
		return driver.switchTo();
	}

}


