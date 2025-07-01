# SQA Engineer Portfolio: E-Commerce Testing

Welcome to my Software Quality Assurance (SQA) portfolio! This public GitHub repository showcases my expertise in automated testing, performance testing, test planning, and bug reproduction through a comprehensive testing project for a demo e-commerce website ([SauceDemo](https://www.saucedemo.com/)). The project uses **Cypress** for end-to-end UI testing, **JMeter** for performance testing, and includes detailed test plans and bug reports to demonstrate a complete SQA lifecycle.

---

## Project Overview

This project tests the functionality and performance of the SauceDemo e-commerce website, covering key user flows (login, product selection, cart, checkout) and load testing for 100 concurrent users. The repository includes Cypress test scripts, JMeter test plans, test reports, documentation, and bug reproduction steps.

### Repository Structure

```
SQA-Ecommerce-Portfolio/
├── cypress/
│   ├── e2e/
│   │   ├── login.cy.js
│   │   ├── add_to_cart.cy.js
│   │   ├── checkout.cy.js
│   ├── reports/
│   │   ├── mochawesome-report/
│   │   │   ├── mochawesome.html
│   ├── cypress.config.js
├── jmeter/
│   ├── ecommerce_load_test.jmx
│   ├── reports/
│   │   ├── performance_report.html
│   │   ├── performance_summary.csv
├── docs/
│   ├── test_plan.md
│   ├── bug_reports/
│   │   ├── bug_001_login_failure.md
│   │   ├── bug_002_cart_issue.md
├── README.md
├── .gitignore
├── package.json
├── LICENSE
```

---

## 1. Test Plan Documentation

The test plan outlines the strategy, scope, and objectives for testing the SauceDemo e-commerce website.

### Test Plan

**Objective**: Ensure the website’s functionality, usability, and performance meet user expectations.  
**Scope**:

- Functional testing: Login, product selection, cart, checkout.
- Performance testing: Load and stress scenarios for 100 concurrent users.
- Bug identification and reproduction.

**Tools**:

- **Cypress**: End-to-end UI testing.
- **JMeter**: Performance and load testing.
- **Mochawesome**: Test reporting for Cypress.
- **GitHub**: Bug tracking and documentation.

**Deliverables**:

- Cypress test scripts and Mochawesome reports.
- JMeter test plan and performance reports (HTML/CSV).
- Test plan document and bug reports.

**Test Environment**:

- Browser: Chrome (latest version).
- URL: https://www.saucedemo.com/.
- JMeter: Local setup with 8GB RAM, Java 11.
- Test Data: Username: `standard_user`, Password: `secret_sauce`.

**Schedule**:

- Test Planning: 2 days.
- Test Case Creation: 3 days.
- Test Execution: 5 days.
- Bug Reporting: Ongoing.
- Performance Testing: 2 days.

**Risks**:

- Limited test data for edge cases.
- Performance results may vary based on network conditions.

**Success Criteria**:

- 100% pass rate for critical test cases (login, checkout).
- No critical/high-severity bugs in production flows.
- Application handles 100 concurrent users with <2s response time.

---

## 2. Cypress Test Scripts

Cypress is used for automated end-to-end testing of the website’s UI. Below are sample test scripts for key scenarios.

### Sample Test: `login.cy.js`

```javascript
describe("E-Commerce Login Tests", () => {
  beforeEach(() => {
    cy.visit("https://www.saucedemo.com/");
  });

  it("should login with valid credentials", () => {
    cy.get("#user-name").type("standard_user");
    cy.get("#password").type("secret_sauce");
    cy.get("#login-button").click();
    cy.url().should("include", "/inventory.html");
    cy.get(".inventory_list").should("be.visible");
  });

  it("should show error for invalid credentials", () => {
    cy.get("#user-name").type("invalid_user");
    cy.get("#password").type("wrong_password");
    cy.get("#login-button").click();
    cy.get('[data-test="error"]').should(
      "contain",
      "Username and password do not match"
    );
  });
});
```

### Sample Test: `add_to_cart.cy.js`

```javascript
describe("E-Commerce Cart Tests", () => {
  beforeEach(() => {
    cy.visit("https://www.saucedemo.com/");
    cy.get("#user-name").type("standard_user");
    cy.get("#password").type("secret_sauce");
    cy.get("#login-button").click();
  });

  it("should add item to cart", () => {
    cy.get("#add-to-cart-sauce-labs-backpack").click();
    cy.get(".shopping_cart_badge").should("have.text", "1");
  });
});
```

### Sample Test: `checkout.cy.js`

```javascript
describe("E-Commerce Checkout Tests", () => {
  beforeEach(() => {
    cy.visit("https://www.saucedemo.com/");
    cy.get("#user-name").type("standard_user");
    cy.get("#password").type("secret_sauce");
    cy.get("#login-button").click();
    cy.get("#add-to-cart-sauce-labs-backpack").click();
    cy.get(".shopping_cart_link").click();
    cy.get("#checkout").click();
  });

  it("should complete checkout", () => {
    cy.get("#first-name").type("John");
    cy.get("#last-name").type("Doe");
    cy.get("#postal-code").type("12345");
    cy.get("#continue").click();
    cy.get("#finish").click();
    cy.get(".complete-header").should("contain", "Thank you for your order");
  });
});
```

### Cypress Configuration: `cypress.config.js`

```javascript
const { defineConfig } = require("cypress");

module.exports = defineConfig({
  e2e: {
    baseUrl: "https://www.saucedemo.com/",
    reporter: "mochawesome",
    reporterOptions: {
      reportDir: "cypress/reports/mochawesome-report",
      overwrite: false,
      html: true,
      json: true,
    },
  },
});
```

### Running Cypress Tests

1. Clone the repository: `git clone https://github.com/your-username/SQA-Ecommerce-Portfolio.git`
2. Navigate to the project: `cd SQA-Ecommerce-Portfolio`
3. Install dependencies: `npm install`
4. Run tests: `npx cypress run --reporter mochawesome`
5. View reports: `cypress/reports/mochawesome-report/mochawesome.html`

---

## 3. Test Reports

Test reports are generated for both Cypress and JMeter tests.

### Cypress Reports

- **Location**: `cypress/reports/mochawesome-report/mochawesome.html`
- **Contents**: Pass/fail status, execution time, screenshots for failed tests, detailed logs.
- **Sample Output**: 8/8 test cases passed for login, cart, and checkout.

### JMeter Reports

- **Location**: `jmeter/reports/performance_report.html`, `jmeter/reports/performance_summary.csv`
- **Contents**: Response times, throughput, error rates, graphical analysis.
- **Sample Output**: Average response time of 1.2s for 100 concurrent users, 0% error rate.

---

## 4. JMeter Performance Testing

JMeter is used to simulate load and stress tests on the website.

### Sample JMeter Test Plan: `ecommerce_load_test.jmx`

- **Thread Group**: 100 users, 10s ramp-up, 5 loops.
- **HTTP Requests**:
  - GET `/` (homepage).
  - GET `/inventory.html` (product page).
  - POST `/checkout-step-one.html` (checkout).
- **Assertions**: Response time <2s, no 5xx errors.
- **Listeners**: Aggregate Report, View Results Tree, HTML Dashboard.

### Running JMeter Tests

1. Download and install Apache JMeter from [jmeter.apache.org](https://jmeter.apache.org/).
2. Open `jmeter/ecommerce_load_test.jmx` in JMeter.
3. Run the test: `jmeter -n -t jmeter/ecommerce_load_test.jmx -l jmeter/reports/performance_summary.csv -e -o jmeter/reports/performance_report`
4. View results: `jmeter/reports/performance_report/`

---

## 5. Bug Reproduction

Bugs are documented with clear reproduction steps, expected vs. actual results, and severity.

### Bug Report: `bug_001_login_failure.md`

```markdown
**Bug ID**: 001  
**Title**: Login Fails with Valid Credentials on Edge Browser  
**Severity**: High  
**Environment**: Microsoft Edge (latest), Windows 10  
**Steps to Reproduce**:

1. Navigate to https://www.saucedemo.com/.
2. Enter username: `standard_user` and password: `secret_sauce`.
3. Click the "Login" button.

**Expected Result**: User is redirected to `/inventory.html`.  
**Actual Result**: Error message: "Username and password do not match."  
**Attachments**: Screenshot, browser console log.  
**Status**: Open
```

### Bug Report: `bug_002_cart_issue.md`

```markdown
**Bug ID**: 002  
**Title**: Cart Item Count Not Updating  
**Severity**: Medium  
**Environment**: Chrome (latest), macOS Ventura  
**Steps to Reproduce**:

1. Log in with valid credentials.
2. Add two items to the cart.
3. Check the cart icon.

**Expected Result**: Cart icon shows "2" items.  
**Actual Result**: Cart icon shows "1" item.  
**Attachments**: Video recording, HAR file.  
**Status**: Open
```

---

## 6. Setup Instructions

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/SQA-Ecommerce-Portfolio.git
   ```
2. **Run Cypress Tests**:
   - Install Node.js and npm.
   - Navigate to the project: `cd SQA-Ecommerce-Portfolio`
   - Install dependencies: `npm install`
   - Run tests: `npx cypress run --reporter mochawesome`
   - View reports in `cypress/reports/`.
3. **Run JMeter Tests**:
   - Install JMeter.
   - Open `jmeter/ecommerce_load_test.jmx`.
   - Run: `jmeter -n -t jmeter/ecommerce_load_test.jmx -l jmeter/reports/performance_summary.csv -e -o jmeter/reports/performance_report`
   - View reports in `jmeter/reports/`.
4. **Explore Documentation**:
   - Test plan: `docs/test_plan.md`
   - Bug reports: `docs/bug_reports/`

---

## 7. Technologies Used

- **Cypress**: End-to-end UI testing.
- **JMeter**: Performance and load testing.
- **Mochawesome**: Test reporting for Cypress.
- **GitHub**: Version control and bug tracking.
- **Markdown**: Documentation for test plans and bug reports.

---

## 8. Contact

- **GitHub**: [your-username](https://github.com/your-username)
- **Email**: your.email@example.com
- **LinkedIn**: [your-linkedin-profile](https://www.linkedin.com/in/your-profile)

---

## 9. License

This repository is licensed under the MIT License. See the `LICENSE` file for details.
