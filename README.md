# Purpose of the fork

This fork of rsc.io/pdf extends the package API with:

  - Implement the method GetPlainText() from object Page. Use to get plain text content (without format)

## How to read all text from PDF:

I write an example function to read file from PATH and return the content of PDF

```golang
package main

import (
	"bytes"
	"fmt"

	"github.com/ledongthuc/pdf"
)

func main() {
	content, err := readPdf("test.pdf") // Read local pdf file
	if err != nil {
		panic(err)
	}
	fmt.Println(content)
	return
}

func readPdf(path string) (string, error) {
	r, err := pdf.Open(path)
	if err != nil {
		return "", err
	}
	totalPage := r.NumPage()

	var textBuilder bytes.Buffer
	for pageIndex := 1; pageIndex <= totalPage; pageIndex++ {
		p := r.Page(pageIndex)
		if p.V.IsNull() {
			continue
		}
		textBuilder.WriteString(p.GetPlainText("\n"))
	}
	return textBuilder.String(), nil
}
```

## Demo
![Run example](https://i.gyazo.com/01fbc539e9872593e0ff6bac7e954e6d.gif)
