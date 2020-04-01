# Fast logging setup for your project

[Back to Table of Contents](../Readme.md)

To setup logging for your project, put the following code somewhere near the start

```nim
    import asyncdispatch, logging
    
    let logger = newFileLogger("normal.log", fmtStr = "[$datetime] [$levelname] ")
    addHandler(logger)
    addTimer(30_000, false, proc(_:AsyncFD): bool = logger.file.flushFile)

    discard stderr.reopen("error.log", fmAppend)
```

Now you can use things like `info("the sky is blue")`, `warn("this special warning comes form Mars")`, `fatal("now you've done it!")` from
anywhere in your program.

We need to flush regularly to this file because by default only `fail` and `warn` get flushed immediately but not `info` and the other
log levels.

We've also redirected `stderr` to a file output. This is helpful when we run the project in the background and an exception gets thrown,
we'll know about it.
