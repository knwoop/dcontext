# xcontext
Copy context.Context without setting timeout

## example

```go
package main

import (
	"context"
	"fmt"
	"time"
  
  "github.com/knwoop/dcontext"
)

func main() {
	parentCtx := context.Background()
	parentCtx = context.WithValue(parentCtx, "trace-id", "trace123")

	ctx, cancel := context.WithCancel(parentCtx)

	// detatch context
	dtx := dcontext.Detach(ctx)

	// Cancel context
	cancel()

	fmt.Println("dtx.Err():", dtx.Err())
	fmt.Println(`dtx.Value("trace-id"):`, dtx.Value("trace-id"))

	// Create a child context with a deadline.
	ttx, cancel := context.WithTimeout(dtx, time.Millisecond)
	cancel()
	deadline, ok := ttx.Deadline()
	fmt.Println("ttx.Deadline():", deadline, ok)
	fmt.Println("ttx.Err():", ttx.Err())
	fmt.Println(`ttx.Value("trace-id"):`, ttx.Value("trace-id"))
}
```
