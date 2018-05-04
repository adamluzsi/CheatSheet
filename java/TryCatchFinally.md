# Try, Catch, Finally

## try with resource

The try-with-resources statement is a try statement that declares one or more resources.
A resource is an object that must be closed after the program is finished with it.
The try-with-resources statement ensures that each resource is closed at the end of the statement.
Any object that implements java.lang.AutoCloseable, which includes all objects which implement java.io.Closeable, can be used as a resource.

The following example reads the first line from a file.
It uses an instance of BufferedReader to read data from the file.
BufferedReader is a resource that must be closed after the program is finished with it:

```java
static String readFirstLineFromFile(String path) throws IOException {
    try (BufferedReader br =
                   new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```

In this example, the resource declared in the try-with-resources statement is a BufferedReader.
The declaration statement appears within parentheses immediately after the try keyword.
The class BufferedReader, in Java SE 7 and later, implements the interface java.lang.AutoCloseable.
Because the BufferedReader instance is declared in a try-with-resource statement,
it will be closed regardless of whether the try statement completes normally or abruptly.
As a result of the method BufferedReader.readLine throwing an IOException).

## 9 Best Practices to Handle Exceptions in Java

### Clean up Resources in a Finally Block or Use a Try-With-Resource Statement

```java
public void closeResourceInFinally() {
	FileInputStream inputStream = null;
	try {
		File file = new File("./tmp.txt");
		inputStream = new FileInputStream(file);

		// use the inputStream to read a file

	} catch (FileNotFoundException e) {
		log.error(e);
	} finally {
		if (inputStream != null) {
			try {
				inputStream.close();
			} catch (IOException e) {
				log.error(e);
			}
		}
	}
}
```

#### Java 7’s Try-With-Resource Statement

```java
public void automaticallyCloseResource() {
	File file = new File("./tmp.txt");
	try (FileInputStream inputStream = new FileInputStream(file);) {
		// use the inputStream to read a file

	} catch (FileNotFoundException e) {
		log.error(e);
	} catch (IOException e) {
		log.error(e);
	}
}
```

### Prefer Specific Exceptions

The more specific the exception is that you throw, the better. Always keep in mind that a co-worker who doesn’t know your code, or maybe you in a few months, need to call your method and handle the exception.

Therefore make sure to provide them as many information as possible. That makes your API easier to understand. And as a result, the caller of your method will be able to handle the exception better or avoid it with an additional check.

So, always try to find the class that fits best to your exceptional event, e.g. throw a NumberFormatException instead of an IllegalArgumentException. And avoid throwing an unspecific Exception.

```java
public void doNotDoThis() throws Exception { ... }

public void doThis() throws NumberFormatException { ... }
```

### Document the Exceptions You Specify

Whenever you specify an exception in your method signature, you should also document it in your Javadoc. That has the same goal as the previous best practice: Provide the caller as many information as possible so that he can avoid or handle the exception.

So, make sure to add a @throws declaration to your Javadoc and to describe the situations that can cause the exception.

```java
/**
* This method does something extremely useful ...
*
* @param input
* @throws MyBusinessException if ... happens
*/
public void doSomething(String input) throws MyBusinessException { ... }
```

### Throw Exceptions With Descriptive Messages

### Catch the most specific exception first

```java
public void catchMostSpecificExceptionFirst() {
	try {
		doSomething("A message");
	} catch (NumberFormatException e) {
		log.error(e);
	} catch (IllegalArgumentException e) {
		log.error(e)
	}
}
```

### Don’t catch with Throwable class

Throwable is the superclass of all exceptions and errors. You can use it in a catch clause, but you should never do it!

### Don’t catch exceptions and than not handle them by ignoring

Have you ever analyzed a bug report where only the first part of your use case got executed?

That’s often caused by an ignored exception. The developer was probably pretty sure that it would never be thrown and added a catch block that doesn’t handle or logs it. And when you find this block, you most likely even find one of the famous “This will never happen” comments.

```java
public void doNotIgnoreExceptions() {
	try {
		// do something
	} catch (NumberFormatException e) {
		// this will never happen
	}
}
```

### Only catch an exception if you want to handle it! Logging and rethrow the exception is not handling it

That is probably the most often ignored best practice in this list. You can find lots of code snippets and even libraries in which an exception gets caught, logged and rethrown.

try {
	new Long("xyz");
} catch (NumberFormatException e) {
	log.error(e);
	throw e;
}
It might feel intuitive to log an exception when it occurred and then rethrow it so that the caller can handle it appropriately. But it will write multiple error messages for the same exception.

```text
17:44:28,945 ERROR TestExceptionHandling:65 - java.lang.NumberFormatException: For input string: "xyz"
Exception in thread "main" java.lang.NumberFormatException: For input string: "xyz"
	at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
	at java.lang.Long.parseLong(Long.java:589)
	at java.lang.Long.(Long.java:965)
	at com.stackify.example.TestExceptionHandling.logAndThrowException(TestExceptionHandling.java:63)
	at com.stackify.example.TestExceptionHandling.main(TestExceptionHandling.java:58)
```

The additional messages also don’t add any information. As explained in best practice #4, the exception message should describe the exceptional event. And the stack trace tells you in which class, method, and line the exception was thrown.

If you need to add additional information, you should catch the exception and wrap it in a custom one. But make sure to follow best practice number 9.

public void wrapException(String input) throws MyBusinessException {
	try {
		// do something
	} catch (NumberFormatException e) {
		throw new MyBusinessException("A message that describes the error.", e);
	}
}

So, only catch an exception if you want to handle it. Otherwise, specify it in the method signature and let the caller take care of it.

### Wrap the Exception Without Consuming it

It’s sometimes better to catch a standard exception and to wrap it into a custom one. A typical example for such an exception is an application or framework specific business exception. That allows you to add additional information and you can also implement a special handling for your exception class.

When you do that, make sure to set the original exception as the cause. The Exception class provides specific constructor methods that accept a Throwable as a parameter. Otherwise, you lose the stack trace and message of the original exception which will make it difficult to analyze the exceptional event that caused your exception.

```java
public void wrapException(String input) throws MyBusinessException {
	try {
		// do something
	} catch (NumberFormatException e) {
		throw new MyBusinessException("A message that describes the error.", e);
	}
}
```

# Summary

As you’ve seen, there are lots of different things you should consider when you throw or catch an exception.
Most of them have the goal to improve the readability of your code or the usability of your API.

Exceptions are most often an error handling mechanism and a communication medium at the same time.
You should, therefore, make sure to discuss the Java exception handling best practices and rules you want to apply with your coworkers so that everyone understands the general concepts and uses them in the same way.

When using Retrace APM with code profiling, you can collect exceptions directly from Java, without any code changes!
