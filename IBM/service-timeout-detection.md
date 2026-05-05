# Service Timeout Detection
- **Company:** IBM
- **Difficulty:** Medium
- **Pattern:** Grouping + Sorting
- **Link:** https://www.fastprep.io/problems/ibm-service-timeout-detection

## Problem
Heartbeat events are recorded with a timestamp and a service identifier. A service times out
if the gap between any two consecutive heartbeats for that service is strictly greater than a
given threshold. Return all service identifiers that time out at least once, sorted lexicographically.

## Key Observations
- Each service has its own independent sequence of heartbeats — gaps must be checked within each service separately
- Timestamps must be sorted per service before checking gaps (input order is not guaranteed to be chronological)
- A service only needs to time out once to be included in the result

## Python
```python
from collections import defaultdict

class Solution:
    def detectTimedOutServices(self, timestamps: List[int], serviceIds: List[str], threshold: int) -> List[str]:
        # group timestamps by service ID
        service_times = defaultdict(list)
        for t, s in zip(timestamps, serviceIds):
            service_times[s].append(t)

        res = []
        for service, times in service_times.items():
            times.sort()  # must sort — input order is not chronological
            for i in range(1, len(times)):
                if times[i] - times[i-1] > threshold:
                    res.append(service)
                    break  # only need to time out once

        return sorted(res)  # lexicographic order
```

## Why sort per service?
The timestamps in the input are not guaranteed to be in chronological order for each service.
Without sorting, you may compare a later heartbeat with an earlier one and get incorrect gaps.
`abs()` is not a substitute — it can produce false positives when events are out of order.
