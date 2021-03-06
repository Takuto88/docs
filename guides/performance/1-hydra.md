# ORY Hydra Performance Benchmarks

In this document you will find benchmark results for different endpoints of ORY Hydra. All benchmarks are executed
using [rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks run against the in-memory storage
adapter of ORY Hydra. These benchmarks represent what performance you would get with a zero-overhead database implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the performance greatly differs between
deployments (e.g. request latency, database configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using flows such as the OAuth 2.0 Client Credentials
Grant, ORY Hydra validates the client credentials using BCrypt which causes (by design) CPU load. CPU load and performance
depend on the BCrypt cost which can be set using the environment variable `BCRYPT_COST`. For these benchmarks,
we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	0.9836 secs
  Slowest:	0.0404 secs
  Fastest:	0.0002 secs
  Average:	0.0095 secs
  Requests/sec:	10166.8123
  
  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.004 [2069]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.008 [1666]	|■■■■■■■■■■■■■■■■■■■■■■
  0.012 [3067]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.016 [2467]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.020 [477]	|■■■■■■
  0.024 [154]	|■■
  0.028 [46]	|■
  0.032 [45]	|■
  0.036 [6]	|
  0.040 [2]	|


Latency distribution:
  10% in 0.0014 secs
  25% in 0.0054 secs
  50% in 0.0106 secs
  75% in 0.0127 secs
  90% in 0.0153 secs
  95% in 0.0175 secs
  99% in 0.0242 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0002 secs, 0.0404 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0127 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0222 secs
  resp wait:	0.0090 secs, 0.0001 secs, 0.0380 secs
  resp read:	0.0002 secs, 0.0000 secs, 0.0147 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	24.4359 secs
  Slowest:	1.1186 secs
  Fastest:	0.0205 secs
  Average:	0.2378 secs
  Requests/sec:	409.2337
  
  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.021 [1]	|
  0.130 [1813]	|■■■■■■■■■■■■■■■■■
  0.240 [4270]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.350 [2602]	|■■■■■■■■■■■■■■■■■■■■■■■■
  0.460 [895]	|■■■■■■■■
  0.570 [255]	|■■
  0.679 [105]	|■
  0.789 [38]	|
  0.899 [12]	|
  1.009 [6]	|
  1.119 [3]	|


Latency distribution:
  10% in 0.1076 secs
  25% in 0.1828 secs
  50% in 0.2102 secs
  75% in 0.2956 secs
  90% in 0.3840 secs
  95% in 0.4137 secs
  99% in 0.6070 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0205 secs, 1.1186 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0087 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0707 secs
  resp wait:	0.2373 secs, 0.0205 secs, 1.1185 secs
  resp read:	0.0002 secs, 0.0000 secs, 0.0782 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading from  which negatively impacts
performance. Performance will be better if IDs and secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.6867 secs
  Slowest:	0.0282 secs
  Fastest:	0.0002 secs
  Average:	0.0067 secs
  Requests/sec:	14562.0497
  
  Total data:	3930000 bytes
  Size/request:	393 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [2430]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.006 [1868]	|■■■■■■■■■■■■■■■■■■■■■
  0.009 [1328]	|■■■■■■■■■■■■■■■
  0.011 [3493]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.014 [657]	|■■■■■■■■
  0.017 [135]	|■■
  0.020 [40]	|
  0.023 [24]	|
  0.025 [19]	|
  0.028 [5]	|


Latency distribution:
  10% in 0.0004 secs
  25% in 0.0031 secs
  50% in 0.0073 secs
  75% in 0.0095 secs
  90% in 0.0111 secs
  95% in 0.0127 secs
  99% in 0.0164 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0002 secs, 0.0282 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0075 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0107 secs
  resp wait:	0.0055 secs, 0.0001 secs, 0.0263 secs
  resp read:	0.0005 secs, 0.0000 secs, 0.0104 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.5658 secs
  Slowest:	0.0246 secs
  Fastest:	0.0002 secs
  Average:	0.0054 secs
  Requests/sec:	17673.3141
  
  Total data:	3910000 bytes
  Size/request:	391 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [2319]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [2785]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.008 [2603]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.010 [1528]	|■■■■■■■■■■■■■■■■■■■■■■
  0.012 [361]	|■■■■■
  0.015 [164]	|■■
  0.017 [138]	|■■
  0.020 [68]	|■
  0.022 [30]	|
  0.025 [3]	|


Latency distribution:
  10% in 0.0005 secs
  25% in 0.0027 secs
  50% in 0.0050 secs
  75% in 0.0072 secs
  90% in 0.0096 secs
  95% in 0.0113 secs
  99% in 0.0174 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0002 secs, 0.0246 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0071 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0094 secs
  resp wait:	0.0026 secs, 0.0001 secs, 0.0206 secs
  resp read:	0.0014 secs, 0.0000 secs, 0.0134 secs

Status code distribution:
  [200]	10000 responses



```
