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
* `s, old := newScanState(r, true, false)`: get a ss struct which newline do not terminates scan and newline count as white space
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
    * (?) Where the old return?
* `n, err = s.doScan(a)` scan one by one without format string
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
## func Scanln
``` go
// Scanln is similar to Scan, but stops scanning at a newline and
// after the final item there must be a newline or EOF.
func Scanln(a ...interface{}) (n int, err error) {
	return Fscanln(os.Stdin, a...)
}
```
## func Fscanln
``` go
// Fscanln is similar to Fscan, but stops scanning at a newline and
// after the final item there must be a newline or EOF.
func Fscanln(r io.Reader, a ...interface{}) (n int, err error) {
	s, old := newScanState(r, false, true)
	n, err = s.doScan(a)
	s.free(old)
	return
}
```
* `s, old := newScanState(r, false, true)` get a ss struct which newline terminates scan and newline do not count as white space
## func Scanf 
``` go
// Scanf scans text read from standard input, storing successive
// space-separated values into successive arguments as determined by
// the format. It returns the number of items successfully scanned.
// If that is less than the number of arguments, err will report why.
// Newlines in the input must match newlines in the format.
// The one exception: the verb %c always scans the next rune in the
// input, even if it is a space (or tab etc.) or newline.
func Scanf(format string, a ...interface{}) (n int, err error) {
	return Fscanf(os.Stdin, format, a...)
}
```
## func Fscanf
``` go
// Fscanf scans text read from r, storing successive space-separated
// values into successive arguments as determined by the format. It
// returns the number of items successfully parsed.
// Newlines in the input must match newlines in the format.
func Fscanf(r io.Reader, format string, a ...interface{}) (n int, err error) {
	s, old := newScanState(r, false, false)
	n, err = s.doScanf(format, a)
	s.free(old)
	return
}
```
* `s, old := newScanState(r, false, false)` get a ss struct which newline do not terminate scan and newline do not count as white space
* `n, err = s.doScanf(format, a)` scan with format string
## func Sscan
``` go
// Sscan scans the argument string, storing successive space-separated
// values into successive arguments. Newlines count as space. It
// returns the number of items successfully scanned. If that is less
// than the number of arguments, err will report why.
func Sscan(str string, a ...interface{}) (n int, err error) {
	return Fscan((*stringReader)(&str), a...)
}
```
* `return Fscan((*stringReader)(&str), a...)` 
    * `stringReader` is string type and implement Read method
    * `(*stringReader)(&str)` convert string pointer type to stringReader pointer type because two have the identical underlying type
## func Sscanf
``` go
// Sscanf scans the argument string, storing successive space-separated
// values into successive arguments as determined by the format. It
// returns the number of items successfully parsed.
// Newlines in the input must match newlines in the format.
func Sscanf(str string, format string, a ...interface{}) (n int, err error) {
	return Fscanf((*stringReader)(&str), format, a...)
}
```
## func Sscanln
``` go
// Sscanln is similar to Sscan, but stops scanning at a newline and
// after the final item there must be a newline or EOF.
func Sscanln(str string, a ...interface{}) (n int, err error) {
	return Fscanln((*stringReader)(&str), a...)
}
```
## func Errorf
``` go
// Errorf formats according to a format specifier and returns the string as a
// value that satisfies error.
//
// If the format specifier includes a %w verb with an error operand,
// the returned error will implement an Unwrap method returning the operand. It is
// invalid to include more than one %w verb or to supply it with an operand
// that does not implement the error interface. The %w verb is otherwise
// a synonym for %v.
func Errorf(format string, a ...interface{}) error {
	p := newPrinter()
	p.wrapErrs = true
	p.doPrintf(format, a)
	s := string(p.buf)
	var err error
	if p.wrappedErr == nil {
		err = errors.New(s)
	} else {
		err = &wrapError{s, p.wrappedErr}
	}
	p.free()
	return err
}
```
* type error is defined as
    ``` go
    type error interface {
        Error() string
    }
    ```
* `p.wrapErrs = true` set wrapErrs to true to handle %w verb
* `s := string(p.buf)` convert buffer to string type then assign to s
* `if p.wrappedErr == nil` p.doPrintf would set wrappedErr if record the target of the %w verb
* `err = errors.New(s)` save string to err
* `err = &wrapError{s, p.wrappedErr}` save string and error to err
    ``` go
    type wrapError struct {
        msg string
        err error
    }

    func (e *wrapError) Error() string {
        return e.msg
    }

    func (e *wrapError) Unwrap() error {
        return e.err
    }
    ```
## type Formatter
``` go
// Formatter is implemented by any value that has a Format method.
// The implementation controls how State and rune are interpreted,
// and may call Sprint(f) or Fprint(f) etc. to generate its output.
type Formatter interface {
	Format(f State, verb rune)
}
```
## type GoStringer
``` go
// GoStringer is implemented by any value that has a GoString method,
// which defines the Go syntax for that value.
// The GoString method is used to print values passed as an operand
// to a %#v format.
type GoStringer interface {
	GoString() string
}
```
## type ScanState
``` go
// ScanState represents the scanner state passed to custom scanners.
// Scanners may do rune-at-a-time scanning or ask the ScanState
// to discover the next space-delimited token.
type ScanState interface {
	// ReadRune reads the next rune (Unicode code point) from the input.
	// If invoked during Scanln, Fscanln, or Sscanln, ReadRune() will
	// return EOF after returning the first '\n' or when reading beyond
	// the specified width.
	ReadRune() (r rune, size int, err error)
	// UnreadRune causes the next call to ReadRune to return the same rune.
	UnreadRune() error
	// SkipSpace skips space in the input. Newlines are treated appropriately
	// for the operation being performed; see the package documentation
	// for more information.
	SkipSpace()
	// Token skips space in the input if skipSpace is true, then returns the
	// run of Unicode code points c satisfying f(c).  If f is nil,
	// !unicode.IsSpace(c) is used; that is, the token will hold non-space
	// characters. Newlines are treated appropriately for the operation being
	// performed; see the package documentation for more information.
	// The returned slice points to shared data that may be overwritten
	// by the next call to Token, a call to a Scan function using the ScanState
	// as input, or when the calling Scan method returns.
	Token(skipSpace bool, f func(rune) bool) (token []byte, err error)
	// Width returns the value of the width option and whether it has been set.
	// The unit is Unicode code points.
	Width() (wid int, ok bool)
	// Because ReadRune is implemented by the interface, Read should never be
	// called by the scanning routines and a valid implementation of
	// ScanState may choose always to return an error from Read.
	Read(buf []byte) (n int, err error)
}
```
## type Scanner
``` go
// Scanner is implemented by any value that has a Scan method, which scans
// the input for the representation of a value and stores the result in the
// receiver, which must be a pointer to be useful. The Scan method is called
// for any argument to Scan, Scanf, or Scanln that implements it.
type Scanner interface {
	Scan(state ScanState, verb rune) error
}
```
## type State
``` go
// State represents the printer state passed to custom formatters.
// It provides access to the io.Writer interface plus information about
// the flags and options for the operand's format specifier.
type State interface {
	// Write is the function to call to emit formatted output to be printed.
	Write(b []byte) (n int, err error)
	// Width returns the value of the width option and whether it has been set.
	Width() (wid int, ok bool)
	// Precision returns the value of the precision option and whether it has been set.
	Precision() (prec int, ok bool)

	// Flag reports whether the flag c, a character, has been set.
	Flag(c int) bool
}
```
## type Stringer
``` go
// Stringer is implemented by any value that has a String method,
// which defines the ``native'' format for that value.
// The String method is used to print values passed as an operand
// to any format that accepts a string or to an unformatted printer
// such as Print.
type Stringer interface {
	String() string
}
```
# Reference
* Standard library: https://pkg.go.dev/fmt@go1.17.1
* The Go Programming Language Specification: https://golang.org/ref/spec
