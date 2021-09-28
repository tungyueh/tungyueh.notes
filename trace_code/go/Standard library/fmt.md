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
* io.Writer is a interface with Write method
    ``` go
    type Writer interface {
        Write(p []byte) (n int, err error)
    }
    ```
* newPrinter()
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
    * pp struct
    ``` go
    // pp is used to store a printer's state and is reused with sync.Pool to avoid allocations.
    type pp struct {
        buf buffer

        // arg holds the current item, as an interface{}.
        arg interface{}

        // value is used instead of arg for reflect values.
        value reflect.Value

        // fmt is used to format basic items such as integers or strings.
        fmt fmt

        // reordered records whether the format string used argument reordering.
        reordered bool
        // goodArgNum records whether the most recent reordering directive was valid.
        goodArgNum bool
        // panicking is set by catchPanic to avoid infinite panic, recover, panic, ... recursion.
        panicking bool
        // erroring is set when printing an error string to guard against calling handleMethods.
        erroring bool
        // wrapErrs is set when the format string may contain a %w verb.
        wrapErrs bool
        // wrappedErr records the target of the %w verb.
        wrappedErr error
    }
    ```
* p.doPrintln(a) 
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
    * doPrintln accept []interface{} so argument a do not need to write as a...
    * Although "...interface{}" and "[]interface{}" are the same type, I am confused when to use each and the other
# Reference
* Standard library: https://pkg.go.dev/fmt@go1.17.1
* The Go Programming Language Specification: https://golang.org/ref/spec
