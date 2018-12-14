
A code to publish a property

```
  IOService::publishResource("com.company.property", PropertyObject);
```


A code to register a notifier for an object property

```
    OSDictionary *match_dict = IOService::resourceMatching("com.company.property");
    if (match_dict) {
        vfsRpcNotifier = IOService::addMatchingNotification(gIOPublishNotification,
                                                            match_dict,
                                                            &callback, this);
        match_dict->release();
    }
```

A notifier is being called after ```IOService::publishResource``` called ```IOResources->registerService()```

```
    frame #1: 0xffffff800a612b8d kernel.development`IOService::invokeNotifier(this=0xffffff8015e3e040, notify=0xffffff801b5efb00) at IOService.cpp:3162 [opt]
    frame #2: 0xffffff800a615b28 kernel.development`IOService::probeCandidates(this=0xffffff8015e3e040, matches=0xffffff80161d3f00) at IOService.cpp:3235 [opt]
    frame #3: 0xffffff800a6158cd kernel.development`IOService::doServiceMatch(this=0xffffff8015e3e040, options=<unavailable>) at IOService.cpp:3758 [opt]
    frame #4: 0xffffff800a617366 kernel.development`_IOConfigThread::main(arg=0xffffff8016b81a40, result=<unavailable>) at IOService.cpp:4130 [opt]
    frame #5: 0xffffff8009f1f5c7 kernel.development`call_continuation + 23
```
