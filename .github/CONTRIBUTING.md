# Contribution Guidelines

First of all, thank you for supporting Javacord with your contribution!
This guide will help you to create high quality pull requests.

## Important links

* [Javacord Discord Server](https://discord.gg/Javacord)

## Commit Best Practices

While there are no strict rules for commit messages, it is strongly recommended to read 
[How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/).

## Testing

When you create pull requests, make sure your changes work by testing them.
If your PR is untested, please mark it as a draft.
There are 2 easy ways to test your changes:
1. Create a Test file in the Javacord project `./javacord-core/src/main/java/org/javacord/core/Testfile.java` and put your code in a main method(Be careful to **NOT** add the file to your VCS and commit it!). Then you just need to log in with a bot, and you can verify if your changes are working properly by using your changes.
2. Create a composite build with Gradle.
   1. Create a new Gradle project in the same directory as the Javacord project.
   2. Add `includeBuild("../Javacord")` to you `settings.gradle` in your created project.
   3. Add `implementation("org.javacord:javacord:SNAPSHOT_VERSION")` to your dependencies and replace the version with the one found in `gradle.properties` in Javacord
   4. Refresh your project to load the Gradle changes

## Code Conventions

### Programming practices
When using collections, carefully decide which one is appropriate:
- **Set:** If the collection can not contain duplicates
- **List:** If the elements in the collection have an order (if they also can not contain duplicates, you still need to use a List).
- **Collection:** Use when the collection can contain duplicates and the items do not have an order.

Never return collections directly, instead create a copy of it with i.e. `Collections.unmodifiableList()`

### Code Style
Javacord follows the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) with a few exceptions:

* Column limit: `120` (instead of `100`)
* Block indentation: `4 spaces` (instead of `2 spaces`)
* Having annotations in the same line as the declaration is **not** allowed:
  ```java
  // Incorrect
  @Override public int hashCode() {
      // ...
  }
  
  // Correct
  @Override
  public int hashCode() {
      // ...
  }
  ```
* Ignored exceptions must be called `ignored`:
  ```java
  try {
      int i = Integer.parseInt(response);
      return handleNumericResponse(i);
  } catch (NumberFormatException ignored) {
      // it's not numeric; that's fine, just continue
  }
  return handleTextResponse(response);
  ```
* You are **not** allowed to omit braces!
  ```java
  // Incorrect
  if (condition)
      return;
  ```
  ```java
  // Correct
  if (condition) {
      return;
  }
  ```
* Acronyms must also be written in camel case! Also try to avoid acronyms that are not commonly known.
  ```java
  // Incorrect
  HTTPRequest
  ```
  ```java
  // Correct
  HttpRequest
  ```
* Name your methods so that they are understandable without reading the documentation.
  A perfect example of this are the `getXyzByName(...)` methods in Javacord. 
  Instead of having one meaningless boolean parameter to distinguish case, we have two different methods.
  ```java
  // Would you know if this includes servers called "javacord"?
  Collection<Server> servers = api.getServersByName("Javacord", true);
  ```
  ```java
  // With two methods it is much clearer:
  Collection<Server> servers = api.getServersByName("Javacord");
  Collection<Server> servers = api.getServersByNameIgnoreCase("Javacord");
  ```
* Do **not** use single letter variable names!
  ```java
  // Incorrect
  Server s = api.getServerById(123L);
  ```
  ```java
  // Correct
  Server server = api.getServerById(123L);
  ```
* Files must be encoded in `UTF-8 without BOM`
* Static imports must not be used
* Wildcard imports must not be used
* **All** methods must have JavaDoc comments. This also applies to private methods. JavaDocs have the following format:
```java
/**
 * Sets the foo.
 *
 * @param foo The foo to set.
 */
public void setFoo(Foo foo) {
     this.foo = foo;
}
```
```java
/**
 * Gets the foo with the given bar.
 *
 * @param bar The bar of the foo.
 *            If one line is too short, format the next line like this.
 * @return The foo with the given bar.
 */
public void getFooByBar(Bar bar) {
     return null;
}
```
Other formats are not allowed:
```java
/* DO NOT FORMAT IT LIKE THIS OR WITH ANY OTHER "CUSTOM" FORMAT */
/**
 * Gets the foo with the given bar.
 * 
 * @param bar
 *          - The bar of the foo.
 * @return
 *          - The foo with the given bar.
 */
public Foo getFooByBar(Bar bar) {
    return null;
}
```

It is a good idea to familiarize yourself with the style of the code by browsing the code a bit, especially looking for classes or methods that are similar to the ones you are working on.