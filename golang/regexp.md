Regexp
---

A bunch of random information about go's regex standard library.

### Named subcaptures

By default these aren't grouped or querable.
You can convert them into a sane representation via

```go
import "regexp"

func AllSubGroups(
	regex *regexp.Regexp,
	s string,
) []map[string]string {
	matchingData := regex.FindAllStringSubmatch(s, -1)
	if matchingData == nil || len(matchingData) == 0 {
		return nil
	}
	var output []map[string]string
	for _, interior_slice := range matchingData {
		localMap := make(map[string]string)
		for i, inner_match := range interior_slice {
			localMap[regex.SubexpNames()[i]] = inner_match
		}
		output = append(output, localMap)
	}
	return output
}
```

This doesn't work for repeat group names and unnamed groups cause issues.

But if you have unique group names it -mostly- works
