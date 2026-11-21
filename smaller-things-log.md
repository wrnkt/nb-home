# Smaller Things :: Log

## TODO
- [ ] switch all logging to `@Slf4j` annotations
- [ ] 

## Notes

Some useful stuff is stashed in `origin/feature/benchmark-runner-stopwatch`.
I still didn't make use of `StopWatch` as I intended to. I want to get a feel for more Spring features
so I can utilize them at work as well.


### Enable Testing Log Output

```kotlin
tasks.test {
    testLogging {
        events("passed", "skipped", "failed", "standardOut", "standardError")
        showStandardStreams = true
    }
}
```

I'd prefer to avoid using Logback configuration for more complex logging tasks.
Junit 5 extensions allow finer-grained control within code. 


#### **Example:**

The below snippet shows how annotating with a custom extension can provide access to before & after test callbacks,
and through them the `ExtensionContext`. That likely means access to the test itself, method names, fields, structure, etc.


Presumably `BeforeAllCallback` and `AfterAllCallback` can be applied and invoked separately; maybe even applied to test classes directly. 

It's likely possible to `@Autowire` the test execution context (or something adjacent) as well.

```java

public class TestLoggingExtension implements BeforeAllCallback, AfterAllCallback {

    @Override
    public void beforeAll(ExtensionContext context) {
        String className = context.getRequiredTestClass().getSimpleName();
        String timestamp = DateTimeFormatter
                .ofPattern("yyyy-MM-dd_HH-mm-ss")
                .format(LocalDateTime.now());

        String filename = "build/test-logs/"
                + className
                + "-"
                + timestamp
                + ".log";

        // configure Logback appender here
    }

    @Override
    public void afterAll(ExtensionContext context) {
        // close/remove appender
    }
}
```



```java
// NOTE: driven by annotation only. I wonder how much information it's possible to pass through the `ExtendWith` annotation?
//       I think this is more flexible than it may first appear
@ExtendWith(TestLoggingExtension.class)
class MyServiceTest {

    @Test
    void createsSomething() {
        log.info("Creating something");
    }
}
```
