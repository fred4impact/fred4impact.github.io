# Intro to Restassured

API testing using RestAssured in Java. RestAssured is a popular Java library for testing REST APIs. It simplifies HTTP requests and assertions, making it straightforward to validate responses.

## Prerequisites

    - Java: Make sure Java is installed on your system.
    -  Maven or Gradle: RestAssured can be managed using Maven or Gradle for dependency management.

## Step 1: Set Up RestAssured

To start, add RestAssured to your project dependencies.
Using Maven:

Add this to your pom.xml:
```
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>5.3.0</version> <!-- Check for the latest version -->
    <scope>test</scope>
</dependency>
```

## Step 2: Write Your First API Test

Below are examples demonstrating the basics of RestAssured syntax.

    Basic GET Request

    This sends a simple GET request to an API and validates the response status code.

    ```
    import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

public class APITest {

    public void testGetRequest() {
        given()
            .when()
                .get("https://jsonplaceholder.typicode.com/posts/1")
            .then()
                .statusCode(200) // Validate status code
                .body("userId", equalTo(1)) // Validate userId in response body
                .body("id", equalTo(1))
                .body("title", notNullValue()); // Check if title is not null
    }
}
```
