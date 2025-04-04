# Py2AnyCode
Python code converter.  
## Dependencies  
### pyfiglet
### rich
### openai
## Run it  
`python Py2AnyCode.py -t go -i test.py`  
In test.py, I write:  
```python
print('hello world')

```
A go file named test.go was generated:  
```go
package main

import "fmt"

func main() {
    fmt.Println("hello world")
}

```
