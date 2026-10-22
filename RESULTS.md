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
**Answer:**Each VU sleeps for 1 second and waits for the HTTP response, so the request rate is below 5 req/s.

### Step 0.3 — A fixed number of iterations
- `iterations`: 20
- Observations: The test completed exactly 20 iterations using 5 VUs and stopped automatically.

### Step 0.6 — Thresholds
- Normal run thresholds: FAIL
- Normal run exit code: 99
- Stricter run thresholds: FAIL
- Stricter run exit code: 99

