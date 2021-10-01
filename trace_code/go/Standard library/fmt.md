# fmt
## func Println
``` go
// Println formats using the default formats for its operands and writes to standard output.
// Spaces are always added between operands and a newline is appended.
// It returns the number of bytes written and any write error encountered.
func Println(a ...interface{}) (n int, err error) {
	return Fprintln(os.Stdout, a...)
}
```
* The final parameter with ... is called variadic parameter and is invoked with zero or more arguments
* type ...interface{} is equivalent to type []interface{}
* a... is final argument and is assignable to a slice type []interface{} so no new slice is created
* os.Stdout is type File which has Write method so it implement the io.Write interface
## func Fprintln
``` go
// Fprintln formats using the default formats for its operands and writes to w.
// Spaces are always added between operands and a newline is appended.
// It returns the number of bytes written and any write error encountered.
func Fprintln(w io.Writer, a ...interface{}) (n int, err error) {
	p := newPrinter()
	p.doPrintln(a)
	n, err = w.Write(p.buf)
	p.free()
	return
}
```
* `io.Writer` is a interface with Write method
    ``` go
    type Writer interface {
        Write(p []byte) (n int, err error)
    }
    ```
* `newPrinter()` get a new pp struct with default format
    ``` go
    // newPrinter allocates a new pp struct or grabs a cached one.
    func newPrinter() *pp {
        p := ppFree.Get().(*pp)
        p.panicking = false
        p.erroring = false
        p.wrapErrs = false
        p.fmt.init(&p.buf)
        return p
    }
    ```
* `p.doPrintln(a)` add space between arguments and a newline after the last argument
    ``` go
    // doPrintln is like doPrint but always adds a space between arguments
    // and a newline after the last argument.
    func (p *pp) doPrintln(a []interface{}) {
        for argNum, arg := range a {
            if argNum > 0 {
                p.buf.writeByte(' ')
            }
            p.printArg(arg, 'v')
        }
        p.buf.writeByte('\n')
    }
    ```
    * `doPrintln` accept []interface{} so argument a do not need to write as a...
    * (?) Although "...interface{}" and "[]interface{}" are the same type, I am confused when to use each and the other 
* `n, err = w.Write(p.buf)` written buffer and returns the number of bytes written and the number of bytes error
* `p.free()` free pp struct to avoid an allocation per invocation
## func Print
``` go
// Print formats using the default formats for its operands and writes to standard output.
// Spaces are added between operands when neither is a string.
// It returns the number of bytes written and any write error encountered.
func Print(a ...interface{}) (n int, err error) {
	return Fprint(os.Stdout, a...)
}
```
## func Fprint
``` go
// Fprint formats using the default formats for its operands and writes to w.
// Spaces are added between operands when neither is a string.
// It returns the number of bytes written and any write error encountered.
func Fprint(w io.Writer, a ...interface{}) (n int, err error) {
	p := newPrinter()
	p.doPrint(a)
	n, err = w.Write(p.buf)
	p.free()
	return
}
```
* `p.doPrint(a)` only add space between two non-string arguments
    ``` go
    func (p *pp) doPrint(a []interface{}) {
        prevString := false
        for argNum, arg := range a {
            isString := arg != nil && reflect.TypeOf(arg).Kind() == reflect.String
            // Add a space between two non-string arguments.
            if argNum > 0 && !isString && !prevString {
                p.buf.writeByte(' ')
            }
            p.printArg(arg, 'v')
            prevString = isString
        }
    }
    ```
## func Printf
``` go
// Printf formats according to a format specifier and writes to standard output.
// It returns the number of bytes written and any write error encountered.
func Printf(format string, a ...interface{}) (n int, err error) {
	return Fprintf(os.Stdout, format, a...)
}
```
## func Fprintf
``` go

// Fprintf formats according to a format specifier and writes to w.
// It returns the number of bytes written and any write error encountered.
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error) {
	p := newPrinter()
	p.doPrintf(format, a)
	n, err = w.Write(p.buf)
	p.free()
	return
}
```
* `p.doPrintf(format, a)` write to buf according to specified format
    * Use `continue formatLoop` label and `break simpleFormat` lable to control loop
# Reference
* Standard library: https://pkg.go.dev/fmt@go1.17.1
* The Go Programming Language Specification: https://golang.org/ref/spec
