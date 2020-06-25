+++
title = "Formatting multi-value variables"
type = "docs"
[menu.docs]
weight = 500
+++

# Formatting multi-value variables

Interpolating a variable with multiple values selected is tricky as it is not straight forward how to format the multiple values into a string that
is valid in the given context where the variable is used. Grafana tries to solve this by allowing each data source plugin to
inform the templating interpolation engine what format to use for multiple values.

> **Note:** The **Custom all value** option on the variable ,ust be blank for Grafana to format all values into a single string.

## Multi-value variables with a Graphite data source

Graphite uses glob expressions. A variable with multiple values would, in this case, be interpolated as `{host1,host2,host3}` if
the current variable value was *host1*, *host2*, and *host3*.

## Multi-value variables with a Prometheus or InfluxDB data source

InfluxDB and Prometheus use regex expressions, so the same variable
would be interpolated as `(host1|host2|host3)`. Every value would also be regex escaped if not, a value with a regex control character would
break the regex expression.

## Multi-value variables with an Elastic data source

Elasticsearch uses lucene query syntax, so the same variable would be formatted as `("host1" OR "host2" OR "host3")`. In this case, every value
must be escaped so that the value only contains lucene control words and quotation marks.

## Formatting troubles

Automatic escaping and formatting can cause problems and it can be tricky to grasp the logic is behind it.
Especially for InfluxDB and Prometheus where the use of regex syntax requires that the variable is used in regex operator context.

If you do not want Grafana to do this automatic regex escaping and formatting, then you must turn off the **Multi-value** or **Include All option** options.