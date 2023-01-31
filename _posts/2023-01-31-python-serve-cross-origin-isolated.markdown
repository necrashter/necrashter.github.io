---
title:  "Serve a Cross-Origin Isolated Webpage using Python"
date:   2023-01-31 19:42:00 +0300
permalink: /python-cross-origin-isolated-server
categories:
- webdev
toc: false
excerpt: "Simple Python HTTP server for testing multi-threaded web applications."
---

If you have ever tried to use `python3 -m http.server` to serve a multi-threaded web application, you may have noticed that browser doesn't allow your application to use `SharedArrayBuffer` (which is required for threading) because the webpage is not cross-origin isolated.

You can workaround this problem by using the following script, which is essentially `http.server` but inserts the required headers for cross-origin isolation:
```python
#!/usr/bin/env python3
from http.server import HTTPServer, SimpleHTTPRequestHandler
import sys

class CustomRequestHandler(SimpleHTTPRequestHandler):
    def end_headers(self):
        self.send_header('Cross-Origin-Embedder-Policy', 'require-corp')
        self.send_header('Cross-Origin-Opener-Policy', 'same-origin')
        return super(CustomRequestHandler, self).end_headers()

host = sys.argv[1] if len(sys.argv) > 2 else '0.0.0.0'
port = int(sys.argv[len(sys.argv)-1]) if len(sys.argv) > 1 else 8000

print("Listening on {}:{}".format(host, port))
print("Connect to http://localhost:{}/ for cross-origin isolation".format(port))
httpd = HTTPServer((host, port), CustomRequestHandler)
httpd.serve_forever()
```
In addition to this, **you need to make sure you connect to `localhost:8000`, not `0.0.0.0:8000`**.
Otherwise it won't work.

To check whether the current site is cross-origin isolated, enter the developer console (Ctrl+Shift+K) and type `crossOriginIsolated`. If it returns true, the site is cross-origin isolated.

Based on [this script](https://fpira.com/blog/2020/05/python-http-server-with-cors).
