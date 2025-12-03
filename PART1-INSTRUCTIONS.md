# Part 1: Selenium Test Automation - Complete Guide

## Overview

This document explains how the Selenium test automation was implemented for Retro Paws Emporium web application.

## Project Setup

### 1. Project Structure Created

```
retro-paws-selenium-tests/
├── pom.xml                          # Maven dependencies and build configuration
├── testng.xml                       # TestNG suite configuration
├── src/
│   └── test/
│       └── java/
│           └── com/
│               └── retropaws/
│                   └── tests/
│                       └── RetroPawsTests.java  # All 14 test cases
└── README.md                        # Quick reference guide
```

### 2. Technologies Used

- **Selenium WebDriver 4.15.0** - For browser automation
- **TestNG 7.8.0** - Test framework for organizing and running tests
- **WebDriverManager 5.6.2** - Automatically manages ChromeDriver
- **Maven** - Dependency management and build tool
- **Java 11** - Programming language
- **Headless Chrome** - Browser runs without GUI (required for Jenkins/EC2)

## Test Cases Implemented

### Total: 14 Test Cases

1. **testHomepageLoads** - Verifies homepage loads successfully
2. **testNavigationBarExists** - Checks navigation bar is displayed
3. **testHeroSectionExists** - Validates hero section with headings
4. **testProductsSectionVisible** - Tests products section is visible
5. **testServicesSectionVisible** - Tests services section is visible
6. **testAnimalsSectionVisible** - Tests animals/pets section is visible
7. **testContactFormExists** - Verifies contact form elements exist
8. **testCartButtonExists** - Checks cart functionality is present
9. **testCheckoutPageAccessible** - Tests checkout page can be accessed
10. **testAuthPageAccessible** - Tests login/auth page loads correctly
11. **testAdminPageRequiresAuth** - Verifies admin page handles authentication
12. **test404PageForInvalidRoute** - Tests 404 error page for invalid URLs
13. **testPageResponsiveness** - Tests responsive design on mobile and desktop
14. **testFooterExists** - Verifies footer is present on the page

## Key Features Implemented

### 1. Headless Chrome Configuration

```java
ChromeOptions options = new ChromeOptions();
options.addArguments("--headless");              // Run without GUI
options.addArguments("--no-sandbox");            // Required for Docker/EC2
options.addArguments("--disable-dev-shm-usage"); // Prevent memory issues
options.addArguments("--window-size=1920,1080"); // Set viewport size
```

**Why headless mode?**
- Required for running tests on Jenkins/AWS EC2 (no display)
- Faster test execution
- Better for CI/CD pipelines

### 2. WebDriverManager Integration

```java
WebDriverManager.chromedriver().setup();
```

**Benefits:**
- Automatically downloads correct ChromeDriver version
- No manual driver management needed
- Works across different environments

### 3. Explicit Waits

```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(15));
wait.until(ExpectedConditions.presenceOfElementLocated(By.tagName("body")));
```

**Why use waits?**
- Handles dynamic content loading (React app)
- Prevents test failures due to timing issues
- More reliable than hardcoded sleeps

## How to Run Tests

### Prerequisites

1. Install Java JDK 11 or higher
2. Install Maven
3. Chrome browser installed

### Step 1: Update Application URL

Edit `RetroPawsTests.java` line 28:

```java
private static final String BASE_URL = "http://localhost:5173";
// Change to: "https://your-deployed-app.com"
```

### Step 2: Run Tests

**Using Maven:**
```bash
cd retro-paws-selenium-tests
mvn clean test
```

**View Results:**
- Console output shows each test execution
- Test reports generated in `target/surefire-reports/`

## Test Execution Flow

1. **@BeforeClass setUp()** - Runs once before all tests
   - Sets up ChromeDriver
   - Configures headless mode
   - Initializes WebDriver and waits

2. **@Test methods** - Run in priority order (1-14)
   - Each test is independent
   - Tests verify specific functionality
   - Assertions validate expected behavior

3. **@AfterClass tearDown()** - Runs once after all tests
   - Closes browser
   - Cleans up resources

## Maven POM Configuration Explained

### Dependencies

```xml
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.15.0</version>
</dependency>
```
- Provides WebDriver API for browser automation

```xml
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.8.0</version>
</dependency>
```
- Test framework for annotations and assertions

```xml
<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.6.2</version>
</dependency>
```
- Manages ChromeDriver automatically

### Build Plugins

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.2.2</version>
</plugin>
```
- Runs tests during Maven build
- Generates test reports

## TestNG Configuration

**testng.xml** defines the test suite:

```xml
<suite name="Retro Paws Emporium Test Suite">
    <test name="Retro Paws Web Application Tests">
        <classes>
            <class name="com.retropaws.tests.RetroPawsTests"/>
        </classes>
    </test>
</suite>
```

## Testing Strategy

### 1. UI Component Testing
- Tests verify presence of key UI elements
- Navigation, hero, products, services, footer

### 2. Page Navigation Testing
- Tests verify all routes are accessible
- Homepage, checkout, auth, admin, 404

### 3. Functionality Testing
- Cart functionality exists
- Contact form elements present
- Admin security

### 4. Responsive Design Testing
- Tests different viewport sizes
- Mobile (375x667) and Desktop (1920x1080)

## Next Steps - Part 2

After completing Part 1, you'll:
1. Push this test code to a **separate GitHub repository**
2. Create Jenkins pipeline to run these tests
3. Configure Docker to run tests in container
4. Set up email notifications for test results

## Troubleshooting

**Problem: Tests fail with "connection refused"**
- Solution: Verify BASE_URL is correct and app is running

**Problem: ChromeDriver version mismatch**
- Solution: WebDriverManager handles this automatically

**Problem: Tests timeout**
- Solution: Increase wait time in setUp() method

**Problem: Headless mode issues on Windows**
- Solution: Remove `--headless` for local debugging

## Summary

Part 1 creates a complete Selenium test automation framework with:
- ✅ 14 automated test cases
- ✅ Headless Chrome configuration for CI/CD
- ✅ Maven project structure
- ✅ TestNG framework
- ✅ Automatic driver management
- ✅ Ready for Jenkins integration

This test project will be integrated with Jenkins in Part 2 for continuous testing.
