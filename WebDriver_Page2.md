## Selenium Webdriver Tips

- Tip 3:  if you have a list of options in a select web element and you need to check if it has an option value you’re looking for

```java
	/*WITH STREAMS and anyMatch*/
	public boolean checkIfAnOptionExists(By selectOptionlocator,String searchOptionValue) {
		Select selectElement = new Select(driver.findElement(selectOptionlocator));
		Predicate<WebElement>  searchPredicate = (e)->e.getText().equals(searchOptionValue);
		return selectElement.getOptions().stream().anyMatch(searchPredicate);
		
		
	}

```

- Tip 4: If you have a list of web elements and you wanted to return only the elements which are visible and has a title contains “title1”. This is quite simple to write using Java 8 lambdas.  Inside a lambda function, you can apply your own conditional logic and return the elements you want.

```java
/*Get only the visible elements with specific title from list of all elements with STREAMS*/
public List<WebElement>  allVisibleElements(){
		List<WebElement> myElements = driver.findElements(allElementsLoc);
		

List<WebElement> visibleElementsWithTitle = myElements.stream().filter((e)-> {
			return e.isDisplayed() && e.getAttribute("class").contains("title1");
		})
		.collect(Collectors.toList());
			
return visibleElementsWithTitle;		

}
```



