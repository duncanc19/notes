# HTTP Security Headers



### strict-origin-when-cross-origin


### Content-Security-Policy

An added layer of security that helps to detect and mitigate certain types of attacks, including Cross-Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft, to site defacement, to malware distribution.

Sets where to accept sources from for different formats:
- Can set default-src to set where to accept content from all data types
- self allows just within your repo
- Can add for specific file types like img-src, if you pull pictures from other websites you'll need to list them here 

```html
<meta http-equiv="Content-Security-Policy"
      content="default-src 'self'; img-src https://*; child-src 'none';">
```
   
### Permission Policy

Set whether a browser has permission to run commands from the document.body on your website


### Example of adding security headers to HTTP responses
      
In flask apps, you can add hooks to the server. We added an after-request hook to add the security headers to the response

```python
def setup_application_http_response_headers(dash_app: dash.Dash):
    """Add HTTP headers to the response"""
    server = dash_app.server

    @server.after_request
    def add_headers(response):
        response.headers.add(
            "Content-Security-Policy",
            "default-src 'self' 'unsafe-eval' 'unsafe-inline' data:",
        )
        response.headers.add("X-Content-Type-Options", "nosniff")
        response.headers.add("X-Frame-Options", "DENY")
        response.headers.add("Referrer-Policy", "no-referrer")
        response.headers.add(
            "Permission-Policy",
            "accelerometer=(), ambient-light-sensor=(), autoplay=(), battery=(), camera=(), "
            "cross-origin-isolated=(), display-capture=(), document-domain=(), encrypted-media=()"
            ", execution-while-not-rendered=(), execution-while-out-of-viewport=(), fullscreen=(),"
            " geolocation=(), gyroscope=(), keyboard-map=(), magnetometer=(), microphone=(), "
            "midi=(), navigation-override=(), payment=(), picture-in-picture=(), "
            "publickey-credentials-get=(), screen-wake-lock=(), sync-xhr=(), usb=(), web-share=()"
            ", xr-spatial-tracking=()",
        )
        return response

```
