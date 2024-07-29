# program-to-test-request-headers-status-in-target-website
package main

import (
    "fmt"
    "net/http"
    "time"
)

// List of all possible headers
var headers = map[string]string{
    "A-IM": "feed",
    "Accept": "text/html",
    "Accept-Charset": "utf-8",
    "Accept-Encoding": "gzip, deflate",
    "Accept-Language": "en-US",
    "Accept-Datetime": "Thu, 31 May 2007 20:35:00 GMT",
    "Access-Control-Request-Method": "GET",
    "Access-Control-Request-Headers": "X-Custom-Header",
    "Authorization": "Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==",
    "Cache-Control": "no-cache",
    "Connection": "keep-alive",
    "Content-Length": "348",
    "Content-Type": "application/x-www-form-urlencoded",
    "Cookie": "$Version=1; Skin=new;",
    "Date": "Tue, 15 Nov 1994 08:12:31 GMT",
    "Expect": "100-continue",
    "Forwarded": "for=192.0.2.60;proto=http;by=203.0.113.43",
    "From": "user@example.com",
    "Host": "en.wikipedia.org",
    "If-Match": `"737060cd8c284d8af7ad3082f209582d"`,
    "If-Modified-Since": "Sat, 29 Oct 1994 19:43:31 GMT",
    "If-None-Match": `"737060cd8c284d8af7ad3082f209582d"`,
    "If-Range": `"737060cd8c284d8af7ad3082f209582d"`,
    "If-Unmodified-Since": "Sat, 29 Oct 1994 19:43:31 GMT",
    "Max-Forwards": "10",
    "Origin": "http://www.example-social-network.com",
    "Pragma": "no-cache",
    "Proxy-Authorization": "Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==",
    "Range": "bytes=500-999",
    "Referer": "http://en.wikipedia.org/wiki/Main_Page",
    "TE": "trailers, deflate",
    "Trailer": "Max-Forwards",
    "Transfer-Encoding": "chunked",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3",
    "Upgrade": "websocket",
    "Via": "1.0 fred, 1.1 example.com (Apache/1.1)",
    "Warning": "199 Miscellaneous warning",
    "X-Requested-With": "XMLHttpRequest",
    "DNT": "1",
    "X-Forwarded-For": "client1, proxy1, proxy2",
    "X-Forwarded-Host": "en.wikipedia.org",
    "X-Forwarded-Proto": "https",
    "Front-End-Https": "on",
    "X-Http-Method-Override": "DELETE",
    "X-ATT-DeviceId": "GT-P7320/P7320XXLPG",
    "X-Wap-Profile": "http://wap.samsungmobile.com/uaprof/SGH-I777.xml",
    "Proxy-Connection": "keep-alive",
    "X-UIDH": "...",
    "X-Csrf-Token": "...",
    "X-Request-ID": "...",
    "X-Correlation-ID": "...",
    "X-Api-Key": "your-api-key",
    "X-Session-Id": "1234567890abcdef",
    "X-User-ID": "12345",
    "X-Auth-Token": "abcdef123456",
    "X-Organization-ID": "67890",
    "X-Real-IP": "192.0.2.1",
    "X-App-Version": "1.0.0",
    "X-App-Name": "MyApp",
    "X-Client-Id": "client-12345",
    "X-Client-Secret": "secret-67890",
    "X-Device-ID": "device-1234567890",
    "X-Environment": "production",
    "X-Timestamp": "2024-07-29T12:34:56Z",
    "X-Signature": "HMACSHA256+abcdef123456",
    "X-Transaction-ID": "7890abcdef123456",
    "X-Custom-Header": "custom-value",
    "X-Debug-Token": "debug1234",
    "X-Feature-Flag": "new-feature-enabled",
    "X-Geo-Location": "lat=40.7128;long=-74.0060",
    "X-RateLimit-Limit": "100",
    "X-RateLimit-Remaining": "99",
    "X-RateLimit-Reset": "60",
    "X-Error-Code": "1234",
    "X-Request-Duration": "150ms",
    "X-Frame-Options": "DENY",
    "X-XSS-Protection": "1; mode=block",
    "X-Content-Type-Options": "nosniff",
    "X-Correlation-ID": "correlation-id-12345",
    "X-RateLimit-Status": "rate-limited",
    "X-Powered-By": "Express",
    "X-Client-Geo-Country": "US",
    "X-Referrer-Policy": "no-referrer",
    "X-Login-Attempt": "1",
    "X-App-Token": "app-token-67890",
    "X-Status": "active",
    "X-Request-ID": "request-id-12345",
}

func main() {
    targetURL := "http://your-target-website.com" // Replace with the target website

    client := &http.Client{
        Timeout: 10 * time.Second,
    }

    for header, value := range headers {
        req, err := http.NewRequest("GET", targetURL, nil)
        if err != nil {
            fmt.Printf("Error creating request for header %s: %v\n", header, err)
            continue
        }

        // Set the header
        req.Header.Set(header, value)

        // Send the request
        resp, err := client.Do(req)
        if err != nil {
            fmt.Printf("Error sending request for header %s: %v\n", header, err)
            continue
        }
        defer resp.Body.Close()

        // Check the response status
        if resp.StatusCode == http.StatusOK {
            fmt.Printf("Header %s is available and accepted.\n", header)
        } else {
            fmt.Printf("Header %s is unavailable or blocked (Status Code: %d).\n", header, resp.StatusCode)
        }
    }
}
