# echo-logrusmiddleware

![logrus middleware](/logrus.png)

An adapter (middleware) to make the Golang [Echo web
framework](https://github.com/labstack/echo) logging work with
[logrus](https://github.com/Sirupsen/logrus), an excellent logging solution.

Improves upon sandalwing/echo-logrusmiddleware by:
1. Using the correct dependencies
2. Including the request_id prop in the log output in order to support echo's request ID middleware.
3. Supporting logging the full request/response body

## Install

```
$ go get github.com/bakatz/echo-logrusmiddleware
```

## Usage

```go
package main

import (
	"github.com/sirupsen/logrus"
	"github.com/labstack/echo"
	"github.com/bakatz/echo-logrusmiddleware"
)

func main() {
	e := echo.New()

	e.Logger = logrusmiddleware.Logger{Logger: logrus.StandardLogger()}

	// to set up request logging using the default parameters (basic fields logged, but no request/response bodies)
	e.Use(logrusmiddleware.Hook())

	// to define your own config
	config := &logrusmiddleware.Config{
		IncludeRequestBodies = true,
		IncludeResponseBodies = true,
	}
	e.Use(logrusmiddleware.HookWithConfig(config))

	// do the rest of your echo setup, routes, listen on server, etc..
}
```
