# K6 Lab Results

> Name: Nitichot Chaiyasit
> Student ID: 6787048
> Date: 22 October 2026

## Part 0

### Step 0.1 — Your first test
- `http_req_duration`: avg=288.46ms min=288.46ms med=288.46ms max=288.46ms p(90)=288.46ms p(95)=288.46ms
- What happened?: The HTTP request succeeded with status 200, and the response time was 288.46 ms.

### Step 0.2 — More users for longer
- Total requests (`http_reqs`): 76
- Request rate (`http_reqs`, requests/second): 3.577965/s
- `http_req_duration p(95)`: 322.76 ms

**Q: Why is the request rate below 5 requests/second even with 5 VUs?**
**Answer:** Each VU sleeps for 1 second and waits for the HTTP response, so the request rate is below 5 req/s.

### Step 0.3 — A fixed number of iterations
- `iterations`: 20
- Observations: The test completed exactly 20 iterations using 5 VUs and stopped automatically.

### Step 0.6 — Thresholds
- Normal run thresholds: FAIL
- Normal run exit code: 99
- Stricter run thresholds: FAIL
- Stricter run exit code: 99

### Step 0.7 — From a requirement to thresholds
- Measured p(95): 1660 ms
- Measured p(99): 1960 ms
- p(95) < 1000 ms: FAIL
- p(99) < 1500 ms: FAIL
- Failure rate < 1%: PASS
- Overall result: FAIL
- One number changed to cause a failure: Changed p(95) threshold from 1000 ms to 100 ms
- Exit code after changing the requirement: 99

### Step 0.8 — Bug hunt checkpoint
- Command: `k6 run --vus 5 --duration 20s 08-bug-hunt.js`
- Original checks and their results: 83.33% passed (160/192). No pepperoni failed 8 times, and no knife needed failed 24 times.
- API exploration command: `k6 run explore.js`
- What was wrong with the request? The request used lowercase names ('pepperoni' and 'knife'), but the API requires case-sensitive names ('Pepperoni' and 'Knife').
- What did you change in `08-bug-hunt.js`? Changed 'pepperoni' to 'Pepperoni' and 'knife' to 'Knife'.
- Checks after the fix: PASS — 100% (183/183 checks passed), 0 failed.

**Q: Why could load checks find this issue when one manual request might not?**
**Answer:** Repeated requests return different pizzas, increasing the chance of detecting incorrect ingredients or tools that a single manual request might miss.

## Part A Results

All Part A tests reuse the user journey in `journey.js` (home page → quotes → vegetarian pizza recommendation).

| Step | Test type | Users (max VUs) | Req/s | p(95) | Max | Failed | Checks |
| ---- | --------- | --------------- | ----- | ----- | --- | ------ | ------ |
| A1 | Smoke | 1 | 0.178549 | 13050 ms | 16540 ms | 0.00 % | 100.00 % |
| A2 | Average load | 3 | 1.21308 | 1260 ms | 2890 ms | 0.00 % | 100.00 % |
| A3 | Stress | 5 | 1.973907 | 1270 ms | 2370 ms | 0.00 % | 100.00 % |
| A4 | Spike | 5 | 0.918053 | 1280 ms | 2030 ms | 0.00 % | 100.00 % |
| A5 | Soak | 2 | 0.899791 | 1180 ms | 2420 ms | 0.00 % | 100.00 % |

### A1 — Smoke test
- All checks passed? YES

### A2 — Average-load test
- Stages: 30s to 3 VUs → 2m at 3 VUs → 30s to 0 VUs.
- Home-page p(95) threshold < 1000 ms: FAIL
- Recommendation p(95) threshold < 1500 ms: PASS
- Notes from HTML report: The overall p(95) was 1.26s. All checks passed (100%), and no HTTP requests failed. However, the home-page response time exceeded its threshold.

### A3 — Stress test
- Stages: 30s to 5 VUs → 2m at 5 VUs → 30s to 0 VUs.
- Overall p(95): 1270 ms
- A2 p(95): 1260 ms
- Compare total `http_req_duration p(95)` with A2: Slightly increased by 10 ms.
- Notes: All checks passed (100%), and the HTTP failure rate was 0%. However, the home-page p(95) exceeded the 1000 ms threshold, so the overall test failed.

### A4 — Spike test
- Stages: 30s at 1 VU → 5s to 5 VUs → 30s at 5 VUs → 5s to 1 VU → 1m at 1 VU → 10s to 0 VUs.
- Did response time rise during the spike?: YES, it fluctuated and peaked at approximately 1.6 seconds.
- Recovery time after the spike: Approximately 10–15 seconds after the load dropped.
- Notes: All checks passed (100%) with 0% HTTP failures. However, the home-page p(95) exceeded its threshold (1.33s > 1s).

### A5 — Soak test
- Stages: 30s to 2 VUs → 10m at 2 VUs → 30s to 0 VUs.
- First minutes' response time/error rate: Response time fluctuated between approximately 300–2200 ms, with 0% HTTP failures.
- Last minutes' response time/error rate: Response time stabilized around 300–400 ms, with 0% HTTP failures.
- Did performance get worse over time?: NO. Response time became more stable, and the error rate remained at 0%.

## Part B — Reflection

### 1. Fill in the results table for Steps A1 to A5.
**Answer:** Completed in the Part A results table above using actual results from the five runs.

### 2. In Step 0.2, why was the request rate below 5 requests per second with 5 virtual users?
**Answer:** Each VU sends a request, waits for its response, and sleeps for one second in every iteration. These delays make the achieved throughput lower than 5 requests per second for 5 VUs.

### 3. Did response time grow from average-load to stress? Explain using the operational envelope.
**Answer:** A2 p(95) = **[ ] ms**, A3 p(95) = **[ ] ms**. **[STATE WHETHER IT GREW]**. The operational envelope is the load range in which the system can satisfy its performance requirements. **[EXPLAIN WHAT YOUR OBSERVED CHANGE DOES OR DOES NOT SUGGEST; REMEMBER THESE ARE SMALL LOADS ON A PUBLIC SITE.]**

### 4. In the spike test, how long did response time take to return to normal after the spike?
**Answer:** **[NUMBER] seconds** according to `reports/a4-spike.html`. **[EXPLAIN HOW YOU IDENTIFIED BASELINE AND RECOVERY ON THE GRAPH.]**

### 5. In the soak test, did response time or error rate worsen over time? Name a defect that several hours of soak testing could reveal.
**Answer:** **[COMPARE EARLY AND LATE MEASUREMENTS FROM A5]**. An example of a long-duration defect is a **memory leak**, which can gradually increase resource consumption and degrade performance.

### 6. Why use p(95) rather than the average in thresholds?
**Answer:** An average can hide unusually slow requests. p(95) describes the response time met by 95% of requests, so it is more useful for assessing the experience of slower users and enforcing a percentile-based performance requirement.

### 7. Give two reasons public-site internet results may differ from results in a lab.
**Answer:** (1) Internet network latency, routing, and congestion vary between the tester and the public site. (2) Other users may be sending traffic to the same shared QuickPizza service during the test, while a controlled lab may have different server resources and background load.

### 8. Why must you never run a stress, spike or breakpoint test with hundreds of users against a site you do not own?
**Answer:** Large uncontrolled loads can degrade or interrupt other people's access to a service, create excessive costs, and amount to unauthorized disruptive testing. Only conduct high-load tests on systems you own or have written permission to test.

### 9. Give one vague performance requirement for an everyday app, rewrite it as a testable requirement, and show K6 thresholds.
**Everyday app:** **[APP YOU USE]**
**Vague requirement:** "The app should load quickly."
**Testable requirement:** **[DEFINE ACTION/ENDPOINT, USER LOAD, TEST DURATION, RESPONSE-TIME PERCENTILES, AND ACCEPTABLE FAILURE RATE.]**
**K6 thresholds:**

```javascript
thresholds: {
  http_req_duration: ['p(95)<[YOUR_MS_LIMIT]'],
  http_req_failed: ['rate<[YOUR_FAILURE_RATE]'],
}
```