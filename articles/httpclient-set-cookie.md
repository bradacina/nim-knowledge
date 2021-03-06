# httpclient Set-Cookie

[Back to Table Of Contents](../README.md)

Problem:

You make a request to a website where you authenticate and the server sends you back a number of cookies that your client should use when making subsequent requests.


I could not find anything in the [httpclient](https://github.com/nim-lang/Nim/blob/version-1-0/lib/pure/httpclient.nim) or [httpcore](https://github.com/nim-lang/Nim/blob/version-1-0/lib/pure/httpcore.nim) packages to handle this case so I rolled my own.

```nim
import httpclient, tables, strutils

proc setCookies(client: HttpClient, response: Response) =
  if not response.headers.table.hasKey("set-cookie"):
    return

  var cookies: seq[string] = @[]

  for cookie in response.headers.table["set-cookie"]:
    cookies.add(cookie.split(";")[0])
  
  client.headers.table["Cookie"] = @[cookies.join("; ")]
```

There are some weaknesses that need to be addressed still:
- What if the `client` already has some `Cookies` set? We have to ensure that we don't overwrite existing values.
- What if the `client` will make requests to a different domain? We have to ensure we don't leak cookies set by one domain to another domain.

[Back to Table of Contents](../README.md)
