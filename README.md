# kitty

The `kitty` is a configuration parser that can parse configuration files and more.

Compared to the json format, `kitty` is simple and easy to use.

## configuration syntax

Use tab indentation for hierarchical definition.

Basic symbol definition:
```
#			comment
=			simple assignment, key=value
[]			`section`
[[]]			[]`section`, the array of `section`
```

For example:
```
#comment like this
host = example.com
ipaddr = 192.168.1.56
port = 43
compression = on
max_conn = 68182

port_enable = true

order = 98, 652, 31, 599, 566, 12, 208


[monitor]
	enabled = true
	ip = 192.168.1.161
	[MAC]
		mac1 = AA:BB:CC
		mac2 = DD:EE:FF
	port = 3698
	cluster = 127.0.0.1, 192.168.16.163

[portal]
	enabled =true
	ip = 192.168.8.198
	port = 3036
	#array
	[[cluster]]
		addr = 10.0.1.160
		wgh = 20
	[[cluster]]
		addr = 10.12.201.187
		wgh = 10
```

## Getting Started

### install
```
go get github.com:yangeagle/kitty
```
You can also update an already installed version:
```
go get -u github.com:yangeagle/kitty
```

### example config file
config file named `simple.conf` like this:
```
#comment like this
host = example.com
ipaddr = 192.168.1.56
port = 43
compression = on

#comment like this

height = 8848.16, 693.254, 1.230, 996

# google
active = false


#array
cluster = 192.168.8.171, 192.168.8.170, 192.168.8.156


distance = 1896

temprature = 90.88

top_level = 9123456


max_conn = 68182


order = 98, 652, 31, 599, 566, 12, 208
```
### example code

The code like this:
```go
package main

import (
	"fmt"

	"github.com/kitty"
)

type ConfigOption struct {
	Hostname string    `kitty:"host"`
	Addr     string    `kitty:"ipaddr"`
	PortNum  int       `kitty:"port"`
	Height   []float32 `kitty:"height"`
	Active   bool      `kitty:"active"`
	Clusters []string  `kitty:"cluster"`
	Dist     int       `kitty:"distance"`
	Temp     float64   `kitty:"temprature"`
	TopLevel *int      `kitty:"top_level"`
	NumConn  int       `kitty:"max_conn"`
	Order    []int     `kitty:"order"`
}

const configFile = "simple.conf"

func main() {

	confParser := kitty.NewConfig()

	err := confParser.ParseFile(configFile)
	if err != nil {
		fmt.Println("ParseFile failed:", err)
		return
	}

	confOption := new(ConfigOption)

	err = confParser.Unmarshal(confOption)
	if err != nil {
		fmt.Println("Unmarshal failed:", err)
		return
	}

	fmt.Println("Hostname:", confOption.Hostname)
	fmt.Println("Addr:", confOption.Addr)
	fmt.Println("Port:", confOption.PortNum)
	fmt.Println("Height:", confOption.Height)
	fmt.Println("Active:", confOption.Active)
	fmt.Println("Clusters:", confOption.Clusters)
	fmt.Println("Dist:", confOption.Dist)
	fmt.Println("Temp:", confOption.Temp)
	fmt.Println("TopLevel:", *confOption.TopLevel)
	fmt.Println("NumConn:", confOption.NumConn)
	fmt.Println("Order:", confOption.Order)
}
```

For more examples, please refer to the [example](https://github.com/yangeagle/kitty/tree/master/example) directory.
 
