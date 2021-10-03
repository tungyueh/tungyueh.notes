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
## func Sprintln
``` go
// Sprintln formats using the default formats for its operands and returns the resulting string.
// Spaces are always added between operands and a newline is appended.
func Sprintln(a ...interface{}) string {
	p := newPrinter()
	p.doPrintln(a)
	s := string(p.buf)
	p.free()
	return s
}
```
* `s := string(p.buf)` converts bytes to string
## func Sprint
``` go 
// Sprint formats using the default formats for its operands and returns the resulting string.
// Spaces are added between operands when neither is a string.
func Sprint(a ...interface{}) string {
	p := newPrinter()
	p.doPrint(a)
	s := string(p.buf)
	p.free()
	return s
}
```
## func Sprintf
``` go
// Sprintf formats according to a format specifier and returns the resulting string.
func Sprintf(format string, a ...interface{}) string {
	p := newPrinter()
	p.doPrintf(format, a)
	s := string(p.buf)
	p.free()
	return s
}
```
## func Scan
``` go 
// Scan scans text read from standard input, storing successive
// space-separated values into successive arguments. Newlines count
// as space. It returns the number of items successfully scanned.
// If that is less than the number of arguments, err will report why.
func Scan(a ...interface{}) (n int, err error) {
	return Fscan(os.Stdin, a...)
}
```
* `return Fscan(os.Stdin, a...)` pass os.Stdin to Fscan to read from standard input
## func Fscan
``` go
// Fscan scans text read from r, storing successive space-separated
// values into successive arguments. Newlines count as space. It
// returns the number of items successfully scanned. If that is less
// than the number of arguments, err will report why.
func Fscan(r io.Reader, a ...interface{}) (n int, err error) {
	s, old := newScanState(r, true, false)
	n, err = s.doScan(a)
	s.free(old)
	return
}
```
* `r io.Reader` is interface with Read method
    ``` go
    type Reader interface {
        Read(p []byte) (n int, err error)
    }
    ```
* `s, old := newScanState(r, true, false)`: get a ss struct which newline terminates scan and newline do not count as white space
    ``` go
    // newScanState allocates a new ss struct or grab a cached one.
    func newScanState(r io.Reader, nlIsSpace, nlIsEnd bool) (s *ss, old ssave) {
        s = ssFree.Get().(*ss)
        if rs, ok := r.(io.RuneScanner); ok {
            s.rs = rs
        } else {
            s.rs = &readRune{reader: r, peekRune: -1}
        }
        s.nlIsSpace = nlIsSpace
        s.nlIsEnd = nlIsEnd
        s.atEOF = false
        s.limit = hugeWid
        s.argLimit = hugeWid
        s.maxWid = hugeWid
        s.validSave = true
        s.count = 0
        return
    }
    ```
    * `s = ssFree.Get().(*ss)` use type assertion to store value from `ssFree.Get()` to `s` with type `*ss`
    * `if rs, ok := r.(io.RuneScanner); ok {`: use type assertion with addional value to avoid run-time panic occurs
* `n, err = s.doScan(a)` scan one by one withou format string
    ``` go
    // doScan does the real work for scanning without a format string.
    func (s *ss) doScan(a []interface{}) (numProcessed int, err error) {
        defer errorHandler(&err)
        for _, arg := range a {
            s.scanOne('v', arg)
            numProcessed++
        }
        // Check for newline (or EOF) if required (Scanln etc.).
        if s.nlIsEnd {
            for {
                r := s.getRune()
                if r == '\n' || r == eof {
                    break
                }
                if !isSpace(r) {
                    s.errorString("expected newline")
                    break
                }
            }
        }
        return
    }
    ```
# Reference
* Standard library: https://pkg.go.dev/fmt@go1.17.1
* The Go Programming Language Specification: https://golang.org/ref/spec
