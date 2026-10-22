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

