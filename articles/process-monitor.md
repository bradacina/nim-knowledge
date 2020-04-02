# Home grown Process Monitor

[Back to Table of Contents)(../Readme.md)

We would like to run and monitor a process and if it dies simply restart it

```nim
import os, osproc, logging

const MonitoredProc = "bot" # the name of the program that you want to monitor

proc monitor(procName:string) =
    var process: Process

    #setup logging
    let logger = newFileLogger("monitor.log", fmtStr = "[$datetime] [$levelname] ")
    addHandler(logger)
    discard stderr.reopen("monitor.error.log", fmAppend)

    while true:
        if process.isNil or not process.running:
            info "starting monitored process"
            process = startProcess(procName, options= {poParentStreams})
            logger.file.flushFile
        
        sleep(60_000) # how often should we check if the program is still running


when isMainModule:

    monitor(MonitoredProc)
```
