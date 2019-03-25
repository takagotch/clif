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


func cmdProgress(out clif.Output) error {
  pbs := out.ProgressBars()
  pb, _ := pbs.Init("default", 200)
  pbs.Start()
  var wg sync.WaitGroup
  wg.Add(1)
  go func(b clif.ProgressBar) {
    defer wg.Done()
    for i := 0; i < 200; i++ {
      b.Increment()
      <-time.After(time.Millisecond * 100)
    }
  }(pb)
  wg.Wait()
  <-pbs.Finish()
}


func callbackFunction(out clif.Output) {
  table := out.Table(headers, clif.OpenTableStyle)
  table.AddRows(rows)
  fmt.Println(table.Render())
}


var (
  headers := []string{"Name", "Age", "Force"}
  rows = [][]string{
    {
      "<important>Yoda<reset>",
      "Very, very old",
      "Like the uber guy'
    },
    {
      "<important>Luck Skywalker<reset>",
      "Not that old",
      "A bit, but not that much"
    },
    {
      "<important>Anakin Skywalker<reset>",
      "Old dude",
      "He si Luckes father! Was kind of stronger in 1-3, but still failed to" +
        " kill Jar Jar Binks. Not even tried, though. What's with that?"
    },
  }
)

func callbackFunction(out clif.Output) {
  table := out.Table(headers)
  table.AddRows(rows)
  fmt.Println(table.Render())
}


cli := clif.New(..)
cli.SetOutput(cli.NewColorOutput().
  SetFormatter(clif.NewDefaultFormatter(clif.SunburnStyles)))


func callbackFunction(in clif.Input) {
  if in.Confirm("Let's" go it?") {
  }
}


func callbackFunction(in clif.Input) {
  father := in.Choose("Who is your father?", map[string]string{
    "yoda": "The small, green guy",
    "darth": "The one with the smoker voice and the dark cape!",
    "obi": "The old man with the light thingy",
  })
  
  if father == "darth" {
  }
}


func callbackFunciton(in clif.Input) {
  foo := in.Ask("What is a foo", nil)
  
  name := in.Ask("Who are you?", func(v string) error {
    if len(v) > 0 {
      return nil
    } else {
      return fmt.Errorf("Didn't catch that")
    }
  })
  
  count := in.AskRegex("How many? ", regexp.MustCompile(`^[0-9]+$`))
}


type Conf struct {
  Foo string
  Bar string
}

func() main {
  cli := clif.New("my-app", "1.2.3", "My app that does something")
  
  configOpt := clif.NewOption("config", "c", "Path to config file", "/default/config/path.json", true, false).
    SetEnv("MY_APP_CONFIG").
    SetParse(function(name, value string) (string, error) {
      conf := new(Conf)
      if raw, err := ioutil.ReadFile(value); err != nil {
        return "", fmt.Unmarshal("Could not read config file %s: %s", value, err)
      } else if err = json.Unmarshal(raw, conf); err != nil {
        return "", fmt.Errorf("Could not unmarshal config file %s: %s", value, err)
      } else if conf.Foo == "" {
        return "", fmt.Errorf("Config %s is missing \"foo"\", value)
      } else {
        cli.Register(conf)
        return value, nil
      }
    })
  cli.AddDefaultOption(configOpt)
  
  cli.New("anything", "Does anything", func(conf *Conf) {
  })
  cli.Run()
}


opt := clif.NewOption("clint-id", "c", "The client ID", "", true, false).SetEnv("CLIENT_ID")

func callbackFunction(c *clif.Command) {
  if c.Option("my-flag").Bool() {
  }
}

arg := clif.NewArgument("my-int", "An integer", "", true, false).
  SetParse(func(name, value string) (string, error) {
    if _, err := strconv.Atoi(value); err != nil {
      return "", fmt.Errorf("Oops: %s is not an integer: %s", name, err)
    } else {
      return value, nil
    }
  })

opt := clif.NewOption("client-id", "c", "The client ID", "", true, false).
  SetParse(func(name, value string) (string, error) {
    if strings.Index(value, "#") != 0 {
      return fmt.Sprintg("#%s", value), nil
    } else {
      return value, nil
    }
  })


func callbackFunction(c *clif.Command) {
  name := c.Option("name").String()
  others := c.Option("other").Strings()
}

flag := clif.NewFlag("my-flag", "f", "Something ..", false)
flag = clif.NewOption("my-flag", "f", "Something ..", "", false, false).IsFlag()
cmd := clif.NewCommand("hello", "A description", callbackFunciton).AddOption(flag)

cmd := clif.NewCommand("hello", "A description", callBackFunction)
  .NewOption("name", "n", "Name for greeting", "", true, false)

arg := cmd.NewOption("other", "O", "Something ..", "default", false, true)
cmd.AddOption(arg)

func callbackFunction(c *clif.Command) {
  name := c.Argumet("name").String()
  others := c.Argument("other").Strings()
}

cmd := clif.NewCommand("hello", "A description", callbackFunciton)
  .NewArgument("name", "Name for greeting", "", true, false)
  
arg := cmd.NewAgument("other", "Something..",  "default", false, true)
cmd.AddArgument(arg)

cli.RegisterNamed("foo", new(MyFoo)).
  RegisterNamed("bar", 123).
  RegisterNamed("baz", "bla")
  
cli.NewCommand("bar", "Call bar", func (named clif.NamedParameters) {
  asMap := map[string]interface{}(named)
  fmt.Println(asMap["baz"].(string))
})


type MyBar interface {
  Bar() string
}
type myFoo struct {
}
func (m *MyFoo) Bar() string {
  return "bar"
}

func main() {
  cli := clif.New("My App", "1.0.0", "An example applicaiton")
  
  foo := &MyFoo()
  t := reflect.TypeOf((*MyBar)(nil)).Elem()
  cli.Register(t.String(), foo)
  
  cli.NewCommand("bar", "Call bar", func (bar MyBar) {
  })
  cli.Run()
}


type MyFoo struct {
  X int
}

func main() {
  cli := clif.New("My App", "1.0.0", "An example application")
  
  foo := &MyFoo{X: 123}
  cli.Register(foo)
  
  cli.NewCommand("foo", "Call foo", func (foo *MyFoo) {
  })
  
  cli.Run()
}

cmd1 := clif.NewCommand("name", "A description", callBackFunction)
cmd2 := clif.NewCommand("other", "Another description", callBackFunciton2)

package main
import "gopkg.in/ukautz/clif.v1"
func main() {
  clif.New("My App", "1.0.0", "An example application").
    New("hello", "The obligatory hello world", func(out clif.Ouput) {
      out.Printf("Hello World\n")
    }).
    Run()
}
```

```sh
./my-app hello --my-flag
./my-app hello --other bar -n Me -O foo
./my-app hello the-name other1 other2 other3
./app name
./app other
go get gopkg.in/ukautz/clif.v1
./demo
```

```
```


