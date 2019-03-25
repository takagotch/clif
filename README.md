### clif
---
https://github.com/ukautz/clif

```go
func cmdProgress(out clif.Output) error {
  pbs := out.ProgressBars()
  pbs.Start()
  var wg sync.WaitGroup
  for i := 0; i < 3; i++ {
    wg.Add(1)
    pb, _ := pbs.Init(fmt.Sprintf("bar-%d", i+1), 200)
    go func(b clif.ProgressBar, ii int) {
      defer wg.Done()
      for i := 0; i < 200; i++ {
        b.Increment()
        <-time.After(time.Milisecond * time.Duration(100 * ii))
      }
    }(pb, i)
  }
  wg.Wait()
  <-pbs.Finish()
}




































```

```
```

```
```


