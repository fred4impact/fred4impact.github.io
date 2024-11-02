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
### POST Request with JSON Body
This sends a POST request to create a new resource, passing JSON data in the request body.

```
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;
import org.json.JSONObject;

public class APITest {

    public void testPostRequest() {
        JSONObject requestParams = new JSONObject();
        requestParams.put("title", "foo");
        requestParams.put("body", "bar");
        requestParams.put("userId", 1);

        given()
            .header("Content-Type", "application/json")
            .body(requestParams.toString())
            .when()
                .post("https://jsonplaceholder.typicode.com/posts")
            .then()
                .statusCode(201)
                .body("id", notNullValue()) // Check if new resource has an id
                .body("title", equalTo("foo"))
                .body("body", equalTo("bar"));
    }
}

```

### PUT Request to Update Resource
Use PUT to update an existing resource.

```
public void testPutRequest() {
    JSONObject requestParams = new JSONObject();
    requestParams.put("title", "updated title");
    requestParams.put("body", "updated body");
    requestParams.put("userId", 1);

    given()
        .header("Content-Type", "application/json")
        .body(requestParams.toString())
        .when()
            .put("https://jsonplaceholder.typicode.com/posts/1")
        .then()
            .statusCode(200)
            .body("title", equalTo("updated title"))
            .body("body", equalTo("updated body"));
}
```

### DELETE Request to Remove Resource
Use DELETE to delete an existing resource.

```
public void testDeleteRequest() {
    given()
        .when()
            .delete("https://jsonplaceholder.typicode.com/posts/1")
        .then()
            .statusCode(200); // Status code 200 or 204 for successful deletion
}
```

### Step 3: Assertions with Hamcrest Matchers

RestAssured integrates with Hamcrest for making assertions. Some commonly used matchers include:

   -  equalTo: Validates that the response value matches a specific value.
   -  hasItem: Checks that a list contains a specified item.
   - notNullValue: Checks that the response value is not null.
   - containsString: Validates that a string contains a specified substring.

For example, to verify a JSON array contains a specific value:

```
   public void testArrayContainsItem() {
       given()
           .when()
               .get("https://jsonplaceholder.typicode.com/posts")
           .then()
               .statusCode(200)
               .body("title", hasItem("qui est esse"));
   }
```
### Step 4: Running Tests

You can run these tests as JUnit or TestNG test cases by wrapping each method in a 

@Test annotation.

```
import org.junit.Test;

public class APITest {

    @Test
    public void testGetRequest() {
        // Implementation here
    }
}
```

