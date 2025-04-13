This is a common CKAD gotcha — not all test images include networking tools.
🎯 That’s one of those sneaky CKAD exam traps — you assume it’s a networking issue, but it’s really just the test image missing tools.

## 🐳 Useful Kubernetes Test Images

| Image                                | Tools Included              | Use Case                           | Example                                                                                   |
|--------------------------------------|-----------------------------|------------------------------------|-------------------------------------------------------------------------------------------|
| `busybox`                            | Shell, ping, nslookup       | Quick shell tests, basic DNS/net   | `kubectl run test --rm -it --image=busybox --restart=Never -- sh`                        |
| `curlimages/curl`                    | `curl` binary               | HTTP request testing                | `kubectl run curl --rm -it --image=curlimages/curl --restart=Never -- curl http://nginx` |
| `nicolaka/netshoot`                 | curl, dig, tcpdump, iproute2 | Deep network debugging              | `kubectl run netshoot --rm -it --image=nicolaka/netshoot --restart=Never -- bash`        |
| `alpine`                             | Tiny image with shell       | Fast pod spin-up for troubleshooting | `kubectl run alpine --rm -it --image=alpine --restart=Never -- sh`                    |
| `nginx`                              | Web server                  | Service or ingress test target      | `kubectl run nginx --image=nginx --port=80`                                              |
| `httpd`                              | Apache web server           | Web server alternative to nginx     | `kubectl run apache --image=httpd --port=80`                                             |
| `postgres` / `mysql`                 | DB server                   | DB deployments for config testing   | `kubectl run pg --image=postgres -e POSTGRES_PASSWORD=example`                           |
