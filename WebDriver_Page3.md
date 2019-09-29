## Selenium Webdriver Tips

- Tip 5: If you have a list of web elements and you wanted to click on the first web element which is visible & has specific title.  You can easily write this using findFirst of Streams.

Though you can solve this using traditional way of fetching the first index from the list but it looks little confusing as why the author is performing the click action on 0th index. Probably it gives more meaning if you apply Java 8 functional programming style.

```java
	/*Without streams, click on the element*/
	public void clickOnFirstElementWithoutStreams(){
		List<WebElement> myElements = driver.findElements(allElementsLoc);
		myElements.get(0).click();
	}



/*Click on the first visible element with specific title from list of all elements with STREAMS*/
	public void clickOnFirstElement(){
		List<WebElement> myElements = driver.findElements(allElementsLoc);
		myElements.stream().filter((e)-> {
			return e.isDisplayed() && e.getAttribute("class").contains("title1");
		})
		.findFirst().get().click();
	}
```
- Tip 6: Let’s say you have a test to pick only the values greater than 50 from a producer. This producer gives you the data in String format. Hence in this case, you can apply Map & filters and transform the data as integer

```java
/*WITH Map & Filters */
public List<Integer> transformValues() {
		List<String> valuesfromWebElement = Arrays.asList("20","30","40","50");
		  List<Integer> numbersGreaterthan50 = valuesfromWebElement.stream().map((e)->Integer.valueOf(e)).filter((e)->e.intValue()>50).collect(Collectors.toList());
		  return numbersGreaterthan50;
		
}	
