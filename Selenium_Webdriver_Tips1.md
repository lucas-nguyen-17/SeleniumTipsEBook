# Selenium Webdriver Tips 1-2

- Tip 1:  if you have a list of web elements where in you want to pick only the elements whose title attribute starts with ‘O’ .  You can probably achieve this via the traditional way of looping each  web-element but with Java 8 streams you can solve it in a clean & simple way.

```Java
private WebDriver driver;

	private By titleElementLoc;

	private By allElementsLoc;

	/*WITHOUT STREAMS*/
	public List<WebElement> getFiltered_Elements() {		
		List<WebElement> allElements = driver.findElements(allElementsLoc);
		List<WebElement> elementWithSpecificTitle = new ArrayList<>();
		for(WebElement element:allElements) {
			if (element.getAttribute("title").startsWith("O")) {
				elementWithSpecificTitle.add(element);	
			}
		}
		return elementWithSpecificTitle;

	}



	/*WITH STREAMS*/
	public List<WebElement> getFilteredElements() {
		//if you have a list of 100 elements and you wish to collect only elements which has title starting with "o";
		//You can easily do this using stream

		WebElement titleElement = driver.findElement(titleElementLoc);
		Predicate<WebElement> hasTitleWith_O= (e) -> titleElement.getAttribute("title").startsWith("o");		
		List<WebElement> allElements = driver.findElements(allElementsLoc);
		List<WebElement> webelementsTitleStartsWith_O = allElements.stream().filter(hasTitleWith_O).collect(Collectors.toList());
		return webelementsTitleStartsWith_O;

	}
```
- Tip 2:  Apply multiple Filters using Java Streams

```Java
	/*WITH STREAMS And multiple filters*/
	public List<WebElement> getElementsWithMultipleFilters() {
		//if you have a list of 100 elements and you wish to collect only elements which has title starting with "o";
		// and an element has class "jumbotron"
		//You can easily do this using stream

		WebElement titleElement = driver.findElement(titleElementLoc);
		Predicate<WebElement> hasTitleWith_O= (e) -> titleElement.getAttribute("title").startsWith("o");		
		Predicate<WebElement> hasClassWithJumbotron= (e) -> titleElement.getAttribute("class").equalsIgnoreCase("jumbotron");

		List<WebElement> allElements = driver.findElements(allElementsLoc);

		List<WebElement> elementWithMultipleFilters = allElements.stream().
				filter(hasTitleWith_O.and(hasClassWithJumbotron)).collect(Collectors.toList());
		return elementWithMultipleFilters;

	}




