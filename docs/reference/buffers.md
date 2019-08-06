# Buffers

Ambassador enables you to configure streaming buffers and buffers filters.

## Buffer filter max request bytes: `max_request_bytes`

`max_request_bytes` is the maximum request size that the filter will buffer before the connection manager will stop buffering and return a 413 response.

## Buffer filter max request time: `max_request_time`

`max_request_time` is the maximum number of seconds that the filter will wait for a complete request before returning a 408 response.

## Streaming buffer high water mark: `per_connection_buffer_limit_bytes`

`per_connection_buffer_limit_bytes` defines the soft limit on size of the cluster’s connections read and write buffers. If unspecified, an implementation defined default is applied (1MiB).

When using an authentication filter, ambassador configures envoy to buffer rather than stream the request.  You may need to configure this setting when combining large payloads and auth modules.

### Example

The various timeouts are applied onto a `Mapping` resource and can be combined.

```yaml
---
apiVersion: ambassador/v0
kind:  AuthService
name:  authentication
auth_service: "http://10.0.1.2:3000"
path_prefix: "/extauth"
config:
  per_connection_buffer_limit_bytes: 5242880
  buffer:
    max_request_bytes: 16384
    max_request_time: 5000
```
