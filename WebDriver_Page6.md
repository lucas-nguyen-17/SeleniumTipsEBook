### Tips on Locators

- Tip 11: It is recommend to store the locators in the page object class instead of storing in locator properties file.  In most of the automation frameworks, POM aka page object model design pattern is widely used since it is easy to scale & maintain. The page object class drives the behavior of the page. It contains only the locators & methods related to the specific page. 

Storing locators in a properties file is kind of an error prone, not easy to scale & maintain. Let’s say we have created a homepage class which refers to homepage.properties file to store the locators. In the event of a locator change, the user has to copy the element name, open the properties file and search for the element name and change the element locator.  And in an odd case, one of your team member created another locator with the same element name, the previous element key will be overwritten with the new locator you created and resulting in a test failure.


- Tip 12: You can also use StringBuilder/StringBuffer class if you want to send an input string to Webelement sendkeys method. SendKeys method looks for CharSequence variable arguments.
StringBuffer,StringBuilder,String class implements CharSequence class.

```java
For example,
public void typeKeys() {		
  StringBuilder sBuilder = new StringBuilder();
  sBuilder.append("Alice").append("**").append("in").append("**").append('\'').append("wonderland");		
  driver.findElement(elementlocator).sendKeys(sBuilder);		
}
```
