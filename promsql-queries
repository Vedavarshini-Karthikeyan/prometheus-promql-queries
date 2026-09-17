# PromQL


# ========================================
# BASIC METRIC QUERIES
# ========================================

# To get the current data of a metric
<metric>

# Example
http_server_requests_seconds_count


# To get the data for a particular request
http_server_requests_seconds_count{uri="/api/todos"}


# To filter using multiple labels
# Multiple label conditions work as AND condition
http_server_requests_seconds_count{uri="/api/todos", method="GET"}


# To achieve OR condition using regex
http_server_requests_seconds_count{status=~"2..|3.."}


# To get metrics except values matching the pattern
http_server_requests_seconds_count{status!~"2.."}


# To get a metric where the label is not equal to a value
http_server_requests_seconds_count{method!="GET"}


# To get a metric where the label matches a regex
http_server_requests_seconds_count{status=~"2.."}


# ========================================
# TIME QUERIES
# ========================================

# To get the metric at a specific time
<metric> @ <timestamp>

# EX:
http_server_requests_seconds_count @ 1708769930


# To get the metric from 5 minutes ago
http_server_requests_seconds_count offset 5m


# To get metrics from a particular time range
<metric>[<time>]

# EX:
http_server_requests_seconds_count[1h]

# EX:
http_server_requests_seconds_count[5m]


# IMPORTANT:
# [5m] creates a range vector.
# It is normally used with functions such as:
# rate(), irate(), increase(), avg_over_time(), etc.

# EX:
rate(http_server_requests_seconds_count[5m])


########################################


# ========================================
# AGGREGATION FUNCTIONS
# ========================================

# SUM() - to get the total value
sum(http_server_requests_seconds_count)


# To get the total by grouping
sum(http_server_requests_seconds_count) by (method)


# To get the total by multiple groups
sum(http_server_requests_seconds_count) by (method, status)


# AVG() - to get the average value
avg(http_server_requests_seconds_count)


# MAX() - to get the maximum value
max(http_server_requests_seconds_count)


# MIN() - to get the minimum value
min(http_server_requests_seconds_count)


# COUNT() - to count the number of time series
count(http_server_requests_seconds_count)

# IMPORTANT:
# count() counts the number of time series.
# It does NOT count the number of HTTP requests.


########################################


# ========================================
# RATE
# ========================================

# rate() - to calculate the average rate of increase per second
# Used mainly with counters
rate(http_server_requests_seconds_count[5m])


# To calculate request rate by method
sum(rate(http_server_requests_seconds_count[5m])) by (method)


# To calculate request rate by status
sum(rate(http_server_requests_seconds_count[5m])) by (status)


# To calculate request rate by URI
sum(rate(http_server_requests_seconds_count[5m])) by (uri)


########################################


# ========================================
# IRATE
# ========================================

# irate() - to calculate the instant rate of change
# Uses the most recent samples in the selected range
irate(http_server_requests_seconds_count[5m])


# rate() gives a smoother average rate
# irate() gives a more immediate rate


########################################


# ========================================
# INCREASE
# ========================================

# increase() - to get the total increase over a time period
increase(http_server_requests_seconds_count[1h])


# To get the total number of requests in the last 5 minutes
increase(http_server_requests_seconds_count[5m])


########################################


# ========================================
# HTTP REQUEST QUERIES
# ========================================

# To get the total request rate
sum(rate(http_server_requests_seconds_count[5m]))


# To get request rate by URI
sum(rate(http_server_requests_seconds_count[5m])) by (uri)


# To get request rate by method
sum(rate(http_server_requests_seconds_count[5m])) by (method)


# To get request rate by status
sum(rate(http_server_requests_seconds_count[5m])) by (status)


# To get 4xx error rate
sum(rate(http_server_requests_seconds_count{status=~"4.."}[5m]))


# To get 5xx error rate
sum(rate(http_server_requests_seconds_count{status=~"5.."}[5m]))


# To get 2xx and 3xx request rate
sum(rate(http_server_requests_seconds_count{status=~"2..|3.."}[5m]))


# To get 5xx error rate by URI
sum(rate(http_server_requests_seconds_count{status=~"5.."}[5m])) by (uri)


# To get 4xx error rate by URI
sum(rate(http_server_requests_seconds_count{status=~"4.."}[5m])) by (uri)


########################################


# ========================================
# ERROR PERCENTAGE
# ========================================

# To calculate 5xx error percentage
100 *
sum(rate(http_server_requests_seconds_count{status=~"5.."}[5m]))
/
sum(rate(http_server_requests_seconds_count[5m]))


# EX:
# If result is 2.5
# approximately 2.5% of requests returned 5xx responses


########################################


# ========================================
# HISTOGRAM
# ========================================

# Common histogram metrics:
#
# http_server_requests_seconds_bucket
# http_server_requests_seconds_sum
# http_server_requests_seconds_count


# To calculate 95th percentile latency
histogram_quantile(
  0.95,
  sum by (le) (
    rate(http_server_requests_seconds_bucket[5m])
  )
)


# To calculate 99th percentile latency
histogram_quantile(
  0.99,
  sum by (le) (
    rate(http_server_requests_seconds_bucket[5m])
  )
)


# To calculate 95th percentile latency by URI
histogram_quantile(
  0.95,
  sum by (le, uri) (
    rate(http_server_requests_seconds_bucket[5m])
  )
)


# To calculate 95th percentile latency by method
histogram_quantile(
  0.95,
  sum by (le, method) (
    rate(http_server_requests_seconds_bucket[5m])
  )
)


# To calculate 95th percentile latency by URI and method
histogram_quantile(
  0.95,
  sum by (le, uri, method) (
    rate(http_server_requests_seconds_bucket[5m])
  )
)


########################################


# ========================================
# AVERAGE LATENCY
# ========================================

# To calculate average request latency
rate(http_server_requests_seconds_sum[5m])
/
rate(http_server_requests_seconds_count[5m])


# To calculate average latency by URI
sum(rate(http_server_requests_seconds_sum[5m])) by (uri)
/
sum(rate(http_server_requests_seconds_count[5m])) by (uri)


########################################


# ========================================
# IMPORTANT PROMQL CONCEPTS
# ========================================

# Instant Vector
# A metric without a time range
http_server_requests_seconds_count


# Range Vector
# A metric with a time range
http_server_requests_seconds_count[5m]


# Counter
# A counter normally increases over time.
# Examples:
#
# http_server_requests_seconds_count
#
# For counters, commonly use:
#
# rate()
# irate()
# increase()


# Histogram
# Histogram metrics normally contain:
#
# _bucket
# _sum
# _count
#
# Example:
#
# http_server_requests_seconds_bucket
# http_server_requests_seconds_sum
# http_server_requests_seconds_count
#
# histogram_quantile() is commonly used with _bucket
# to calculate percentiles.


########################################


# ========================================
# MOST IMPORTANT QUERIES FOR INTERVIEW
# ========================================

# Total request rate
sum(rate(http_server_requests_seconds_count[5m]))


# Request rate by URI
sum(rate(http_server_requests_seconds_count[5m])) by (uri)


# Request rate by method
sum(rate(http_server_requests_seconds_count[5m])) by (method)


# Request rate by status
sum(rate(http_server_requests_seconds_count[5m])) by (status)


# 4xx error rate
sum(rate(http_server_requests_seconds_count{status=~"4.."}[5m]))


# 5xx error rate
sum(rate(http_server_requests_seconds_count{status=~"5.."}[5m]))


# 5xx error percentage
100 *
sum(rate(http_server_requests_seconds_count{status=~"5.."}[5m]))
/
sum(rate(http_server_requests_seconds_count[5m]))


# 95th percentile latency
histogram_quantile(
  0.95,
  sum by (le) (
    rate(http_server_requests_seconds_bucket[5m])
  )
)


# 99th percentile latency
histogram_quantile(
  0.99,
  sum by (le) (
    rate(http_server_requests_seconds_bucket[5m])
  )
)


# Total requests in the last 1 hour
increase(http_server_requests_seconds_count[1h])


# Average request latency
rate(http_server_requests_seconds_sum[5m])
/
rate(http_server_requests_seconds_count[5m])
