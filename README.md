# Subjack

[![Github](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/PushkraJ99)
[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/intent/follow?screen_name=PushkraJ99) 
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/you_are_not_goodlooking_but_he)
[![Medium](https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white)](https://medium.com/@pushkrajdhuri07)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/pushkaraj-dhuri/)


Subjack is a Subdomain Takeover tool written in Go designed to scan a list of subdomains concurrently and identify ones that are able to be hijacked. With Go's speed and efficiency, this tool really stands out when it comes to mass-testing. Always double check the results manually to rule out false positives.

Subjack will also check for subdomains attached to domains that don't exist (NXDOMAIN) and are **available to be registered**. No need for dig ever again! This is still cross-compatible too.

**What's New? (Last Updated 09/17/18)**
- Custom fingerprint support
- New Services (Re-added Zendesk && Added Readme, Bitly, and more)
- Slight performance enhancements

## Installing

Requires [Go](https://golang.org/dl/)

`go install github.com/PushkraJ99/subjack@latest  `

## How To Use:
```
Create subjack Folder in home/kali/go and Create file fingerprints.json
Copy and Paste Content of fingerprints.json file in home/kali/go/subjack/fingerprints.json
```

Examples: 
- `./subjack -w subdomains.txt -t 100 -timeout 30 -o results.txt -ssl`

Options:
- `-d test.com` if you want to test a single domain.
- `-w domains.txt` is your list of subdomains.
- `-t` is the number of threads (Default: 10 threads). 
- `-timeout` is the seconds to wait before timeout connection (Default: 10 seconds).
- `-o results.txt` where to save results to. For JSON: `-o results.json`
- `-ssl` enforces HTTPS requests which may return a different set of results and increase accuracy.
- `-a` skips CNAME check and sends requests to every URL. **(Recommended)**
- `-m`	flag the presence of a dead record, but valid CNAME entry.
- `-v` verbose. Display more information per each request. 
- `-c` Path to configuration file.

## Practical Use

You can use [scanio.sh](https://gist.github.com/haccer/3698ff6927fc00c8fe533fc977f850f8) which is kind of a PoC script to mass-locate vulnerable subdomains using results from Rapid7's Project Sonar. This script parses and greps through the dump for desired CNAME records and makes a large list of subdomains to check with subjack if they're vulnerable to Hostile Subdomain Takeover. Of course this isn't the only method to get a large amount of data to test. **Please use this responsibly ;)**

## Adding subjack to your workflow

```go
package main

import (
	"fmt"
	"encoding/json"
	"io/ioutil"
	"strings"

	"github.com/PushkraJ99/subjack/subjack"
)
 

func main() {
	var fingerprints []subjack.Fingerprints
	config, _ := ioutil.ReadFile("custom_fingerprints.json")
	json.Unmarshal(config, &fingerprints)

	subdomain := "dead.cody.su"
	/* Use subjack's advanced detection to identify 
	if the subdomain is able to be taken over. */
	service := subjack.Identify(subdomain, false, false, 10, fingerprints)

	if service != "" {
		service = strings.ToLower(service)
		fmt.Printf("%s is pointing to a vulnerable %s service.\n", subdomain, service)
	}
}
```

See the [godoc](https://godoc.org/github.com/haccer/subjack/subjack) for more functions.

## FAQ
**Q:** What should my wordlist look like?

**A:** Your wordlist should include a list of subdomains you're checking and should look something like:
```
assets.cody.su
assets.github.com
b.cody.su
big.example.com
cdn.cody.su
dev.cody.su
dev2.twitter.com
```

## References
Extra information about Hostile Subdomain Takeovers:

- [https://github.com/EdOverflow/can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz)
- [https://labs.detectify.com/2014/10/21/hostile-subdomain-takeover-using-herokugithubdesk-more/](https://labs.detectify.com/2014/10/21/hostile-subdomain-takeover-using-herokugithubdesk-more/)

<br><br>

[![](https://visitcount.itsvg.in/api?id=PushkraJ99&icon=8&color=12)](https://visitcount.itsvg.in)
<p align="center"> 
  <b> Visitor count</b><br>
  <img src="https://profile-counter.glitch.me/PushkraJ99/count.svg" />
</p>
