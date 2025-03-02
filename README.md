filter
======
[![Build Status](https://travis-ci.org/GaoYusong/filter.svg?branch=master)](https://travis-ci.org/GaoYusong/filter)

IP filter library inspired by pcap filter.

The library is stable and strong, the coverage of test is 95.3%.

## How to use it

set your env GOPATH

go get github.com/GaoYusong/filter

## Network address

Network address can be abbreviated, for instance, 192.168.1.0 is the abbreviation for 192.168.1.0/32, 192.168.1 as 192.168.1.0/24, 172.16 as 172.16.0.0/16, 10 as 10.0.0.0/8

## Operator and priority

The priority decreases from top to bottom.
I recommend using parentheses.

level|Operator     | Associativity
-----|-------------|-------------------
1    |not,!          | right
2    |and,&&,or,&#124;&#124; | left

## Example

Is the host a private ip address or in network address 100.0.10.0/24 but not in 100.0.10.128/25?

```Go
package main

import (
	"fmt"
	"github.com/GaoYusong/filter"
)

func main() {
	f := filter.FilterT{}

	err := f.Compile("(10 or 172.16 or 192.168) or (100.0.10 and !100.0.10.128/25)")
	if err != nil {
		panic(err)
	}

	for _, host := range []string{
		"10.0.0.1",
		"172.16.0.1",
		"192.168.0.1",
		"100.0.10.1",
		"100.0.10.129",
		"166.1.1.1",
	} {
		sHost, err := filter.ParseHost(host)
		if err != nil {
			panic(err)
		}
		fmt.Printf("Host %-16s%t\n", host, f.Check(sHost))
	}

}

```

## Lisence
Apache License v2.0

  

