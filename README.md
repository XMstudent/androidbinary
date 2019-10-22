androidbinary
=====

[![Build Status](https://github.com/XMstudent/androidbinary/workflows/Test/badge.svg)](https://github.com/XMstudent/androidbinary/actions)
[![GoDoc](https://godoc.org/github.com/XMstudent/androidbinary?status.svg)](https://godoc.org/github.com/XMstudent/androidbinary)

Android binary file parser

## High Level API

### Parse APK files

``` go
package main

import (
	"github.com/XMstudent/androidbinary/apk"
)

func main() {
	pkg, _ := apk.OpenFile("your-android-app.apk")
	defer pkg.Close()

	icon, _ := pkg.Icon(nil) // returns the icon of APK as image.Image
	pkgName := pkg.PackageName() // returns the package name
}
```

## Low Level API

### Parse XML binary

``` go
package main

import (
	"encoding/xml"

	"github.com/XMstudent/androidbinary"
	"github.com/XMstudent/androidbinary/apk"
)

func main() {
	f, _ := os.Open("AndroidManifest.xml")
	xml, _ := androidbinary.NewXMLFile(f)
	reader := xml.Reader()

	// read XML from reader
	var manifest apk.Manifest
	data, _ := ioutil.ReadAll(reader)
	xml.Unmarshal(data, &manifest)
}
```

### Parse Resource files

``` go
package main

import (
	"fmt"
	"github.com/XMstudent/androidbinary"
)

func main() {
	f, _ := os.Open("resources.arsc")
	rsc, _ := androidbinary.NewTableFile(f)
	resource, _ := rsc.GetResource(androidbinary.ResID(0xCAFEBABE), nil)
	fmt.Println(resource)
}
```

## License

This software is released under the MIT License, see LICENSE.
