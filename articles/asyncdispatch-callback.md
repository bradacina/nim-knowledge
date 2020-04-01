# The Callback type in asyncdispatch and friends

If you take a look at the *asyncdispatch* documentation (and its friends) you'll notice that there exists a Callback type that can't be 
found in the docs. Instead you have to dig into the sources to find

```nim
type
    Callback = proc (fd: AsyncFD): bool {.closure, gcsafe.}
```
