### Q1: Token Expiration

An API token expires in 15 minutes but your test suite takes 30 minutes. How would you handle this?

When an API token expires in 15 minutes but the automation test suite runs for 30 minutes, the framework should handle token renewal automatically to prevent authentication failures during execution. Relying on a single token for the entire suite can make tests unstable and lead to unnecessary failures.

A good solution is to implement an automatic token refresh mechanism in the framework. Before sending API requests, the framework can validate whether the token is still active. If the token is expired or close to expiration, a new token should be generated using the authentication or refresh token API.

Key Approaches

1.Auto-refresh the token

    Detect token expiration automatically.
    enerate a new token without stopping the test execution.

2.Generate tokens dynamically

    Create a fresh token for each test case or test module.
    Improves test independence and reliability.

3.Use setup hooks or utilities

    Centralize token handling in reusable methods.
    Makes maintenance easier.

4.Handle parallel execution carefully

    Ensure multiple threads do not refresh the token simultaneously.

Additionally, logging token refresh activity is useful for debugging failed authentication scenarios. Overall, automating token management is the best approach because it improves test stability, reduces maintenance effort, and supports long-running execution.

### Q2: Status Code Design

An API returns 200 with error messages in the body instead of 4xx/5xx codes. Is this a bug? Should you
report it?

Yes, this can be considered a design issue and should usually be reported. In REST APIs, HTTP status codes are intended to clearly indicate whether a request was successful or failed. Returning 200 OK along with an error message in the response body violates standard API design practices.

For example:

- 401 Unauthorized should be used for authentication failures.
- 400 Bad Request should be returned for invalid inputs.
- 500 Internal Server Error should indicate server-side failures.

Using 200 for error scenarios can create confusion for automation scripts, client applications, and monitoring systems because they may interpret the request as successful.

However, before reporting it as a defect, the tester should verify:

- API documentation
- Business requirements
- Existing system design

Some legacy systems intentionally return 200 for all responses and include custom error codes in the response body. Even in such cases, it is still worth raising as an improvement suggestion because proper HTTP status codes improve API readability, maintainability, and standards compliance.
