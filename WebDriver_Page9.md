### Xpath & Javascript executor

- **Tip 17:** I always noticed that most of SDETs write the xpath of a HTML element using Firebug or any other xpath generation add-ons.
This may not help you to learn and understand xpath concepts thoroughly as you’re always dependent on add-ons. Instead I would recommend to use Google Chrome browser console to locate the HTML elements and write the xpath or CSS locators.
For example, if you wanted to locate a menu [a hamburger menu type] on Walmart.com home page

![alt text](https://github.com/sanmaru/SeleniumTipsEBook/blob/master/walmarthomepage.png)

```javascript

Go to chrome console and write the xpath/css

$x -> evaluates a xpath ,  $$ -> evaluates CSS locator.

$$ & $x returns an array of elements matching to the locator.  
The following example is demonstrates how you can write and evaluate a xpath/css in chrome console


XPATH:
$x("//div[@data-tl-id='header-GlobalHeaderLeftNav']//span[@class='g_a g_h']") 

CSS:
$$("div[data-tl-id='header-GlobalHeaderLeftNav'] span[class='g_a g_h']")


```
![alt text](https://github.com/sanmaru/SeleniumTipsEBook/blob/master/walmarthomepage_ellen.png)


*For more info on chrome console. Refer this: https://developers.google.com/web/tools/chrome-devtools/console/*


- Tip 18:  In a few odd cases, you have may have to inject Javascript into the webdriver if the traditional click or sendkeys methods of Webdriver does not work. Here are few code snippets to inject.

```java
Click action using javascript:
public void clickViaJs(WebElement element) {
		// argumemts[0] means get the first argument from executeScript method
		((JavascriptExecutor)driver).executeScript("arguments[0].click();",element);
}

```
**Set a value for an input text field using javascript**
public void enterTextViaJs(){
String jScript="var eles=document.getElementsByName(\"firstname\");eles[0].value=\"Something\";";
		((JavascriptExecutor)driver).executeScript(jScript);
				
}

```
**Locate an element using CSS locator and return the attribute value via Javascript **
```javascript
document.querySelector is similar to driver.findElement(By.csslocator). 
if you want to all the elements matching to the locator, you can use document.querySelectorAll

public String getAttributeViaJS(String attribute) {
		//Search for an element via CSS selector and get the attribute
		String jScript="document.querySelector(\"a[data-tl-id='categorypage-TempoCategoryTile-0-link']\").getAttribute(\""+attribute+"\")";
		String attributeValue = (String) ((JavascriptExecutor)driver).executeScript(jScript);
		return attributeValue;
		
}
```
**Get all attributes of an HTML element via Javascript.**
```java
public String[] getAllAttributesOfElement(String locatorId){
	String jScript="var ele=document.getElementById(\""+locatorId+"\");\r\n" + 
				"var attrs=ele.attributes; var list=[];\r\n" + 
				"for (var i in attrs){\r\n" + 
				"  list.push(attrs[i]);\r\n" + 
				"}\r\n" + 
				"return list;";
String[] allAttributes = (String[]) ((JavascriptExecutor)driver).executeScript(jScript);
		return allAttributes;
}
```
**Scroll a page to the top via Javascript:**

```java
public void scrollPageToTop() {
		String jScript="window.scrollTo(0,0)";
		((JavascriptExecutor)driver).executeScript(jScript);
			
	}
```
**Highlight a HTML element via javascript**
```java
public void highlightElementViaJScript(String locator) {
		String jScript="var ele=document.getElementById(\"global-search-input\");\r\n" + 
				"ele.style.background=\"yellow\";";
		((JavascriptExecutor)driver).executeScript(jScript);
			
}
```
![alt text](https://github.com/sanmaru/SeleniumTipsEBook/blob/master/highlightpage.png)

*The possibilities of playing around with java script+selenium are many, if you know a bit of javascript.*
