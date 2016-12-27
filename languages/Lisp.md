
# Lisp

- Extension: .lisp
- Interpreted, interactive and functional programming language.
- Optionally compiled to bytecode (.fasl).
- Suggests recursion over iteration.
- Reference: http://www.lispworks.com/documentation/lw50/CLHS/Front/index.htm
- REPL - Read Evaluate Print Loop

## Comments
```lisp
;; Line comments
; Line comments (inline after code)
```

## Compile and Load
```lisp
(compile 'len)             ;; Compiles a single function
(compile-file "file")      ;; Compiles file.lisp to file.fasl
(load "file")              ;; Loads file.fasl or file.lisp
(compile-load "file")      ;; Compiles file and loads it
```

## Data Types
| Data Type              | Example                                    |
|------------------------|--------------------------------------------|
| Integer                | 4                                          |
| Real                   | 4.5                                        |
| Bignum                 | 23525...235  (automatically converted)     |
| Rational               | 1/3 (not 0.333 nor 0.3334)                 |
| Character              | #\a #\space #\x0b87                        |
| String                 | "hello world"                              |
| Symbol (as variable)   | myvar                                      |
| Symbol (as function)   | #'fn                                       |
| Symbol name            | 'mysymbol                                  |
| Keyword                | :hello                                     |
| List                   | '(1 2 3)                                   |
| Empty List             | '() or NIL                                 |
| True                   | T                                          |
| False                  | NIL                                        |


## Dotted Pairs
```lisp
(cons 1 2)        ;; Constructs dotted pair (1 . 2)
(car <pair>)      ;; Returns first element of pair
(cdr <pair>)      ;; Returns second element of pair
```

## Lists
```lisp
(list <el1> <el2> ... <eln>)    ;; New list with these elements
(first <list>)                  ;; Returns first element of list
(rest <list>)                   ;; Returns list without first element
(cons <el> <list>)              ;; Adds el to list in first position
(push <el> <list>)              ;; Destructive cons
(pushnew <el> <list>)           ;; Destructive cons if not in list already
(nth <pos> <list>)              ;; Returns the element in position pos of list
(remove <el> <list>)            ;; Remove el from list
(delete <el> <list>)            ;; Remove el from list desctructively
(append <l1> <l2>)              ;; Concatenate two lists
(nconc <l1> <l2> ...)           ;; Concatenate lists (destructive)
(mapcar <fn> <list>)            ;; Applies fn to all elements in list
(reduce <fn> <list>)            ;; Applies fn to el1 and el2, then to the result and el3, etc, until there's only one result to return
(member <el> <list>)            ;; Returns list after first occurrence of <el> (nil if <el> not present in list)
```

## Property Lists (plists)
```lisp
(getf <plist> <key> &optional <default-val>)   ;; Get value from key
(remf <plist> <key>)                           ;; Remove property from list
```

## Property Lists in symbols
```lisp
(get <sym> <key>)       ;; Access key in symbol 
(remprop <sym> <key>)   ;; Remove property from symbol
```

## Association Lists
```lisp
(acons <key> <datum> '())  ;; New assoc list with one pair key:datum
(assoc <item> <list>)      ;; Gets datum for key
```


## Arrays
```lisp
(make-array '(4))                                ;; Array with 4 positions
(make-array '(5 4))                              ;; Array with 5 rows, 4 columns
(make-array '(4) :initial-element 0)             ;; Fill all positions with 0
(make-array '(4) :initial-contents '(1 2 3 4))   ;; Initialize all values
(array-dimensions <ar>)                          ;; Returns list with dimension lengths
(array-dimension <ar> 0)                         ;; Return length of first dimension
(aref <ar> 0)                                    ;; Returns first element of array
(aref <ar> 0 4)                                  ;; Returns fifth element of first dimension
```

## Hash Tables
```
(make-hash-table)                              ;; Create hash table
(make-hash-table :test #'string.=)             ;; Specify compare function for keys
(make-hash-table :size xxx :rehash-size yyy)   ;; Specify sizes and rehash sizes
(gethash <key> <hashtable>)                    ;; Return hash table entry
(remhash <key> <hashtable>)                    ;; Remove hash table entry
(clrhash <table>)                              ;; Clear hash table
(maphash #'(lambda (key val) ...) <tbl>)   
```

## Structures
```lisp
;; Fields and default values
(defstruct student
    (class 't1)
    (num 57863)
    (name "Mike"))

(make-student [:name "John", ...])     ;; Optional keyword parameters
(student-name s)                       ;; Accessor
(copy-student s)                       ;; Shallow copy
(student-p s)                          ;; Is s a student?
```

## Sequences (Lists, Arrays 1D and Strings)
```lisp
(position <item> <seq>)             ;; Index of item in seq
(elt <seq> <pos>)                   ;; Returns the element in position pos in seq
(copy-seq <seq>)                    ;; Shallow copy
(length <seq>)                      ;; Number of elements
(reverse <seq>)                     ;; Reverse sequence
(nreverse <seq>)                    ;; Reverse sequence (maybe destructive)
(concatenate 'list <s1> <s2> ...)   ;; Concatenate sequences
(fill <seq> <item> :start :end)     ;; Fill seq with item
(remove <item> <seq>)               ;; Remove item from seq
(remove-if <pred> <seq>)            ;; Remove item from seq if pred is T for it
(remove-if-not <pred> <seq>)        ;; Remove item from seq if pred is nil for it
(delete <item> <seq>)               ;; Destructive remove
(substitute <new> <old> <seq>)      ;; Replace old with new in seq
(substitute-if <n> <pred> <s>)      ;; Replace with n when pred is T
(substitute-if-not <n> <pred> <s>)  ;; Replace with n when pred is nil
(find <item> <seq>)                 ;; Returns item if it's in seq, nil otherwise
(find-if <pred> <seq>)              ;; Returns first item that makes pred T
(find-if-not <pred> <seq>)          ;; Returns first item that makes pred nil
(sort <seq> <pred>)                 ;; Ordena in-place (destrutivo)
```

## Variables

### Closures
```lisp
(let ((<var1> <val2>)
      ;; ...
      (<varn> <valn>))
     ;; Here the variables are defined
     ;; ...
     )
```

##### Interdependent variables
```
(let* ((i 0)
       (j (+ 1 i)))
      ;; ...
      )
```

### Assign value to a variable
```lisp
(setf x 1)
(setf (aref a 0) 1)       ;; Works on everything, even array positions
(incf x 1)                ;; Equivalent to (setf x (1+ x)) and (setf x (+ 1 x))
(defc x 1)                ;; Equivalent to (setf x (1- x)) and (setf x (- 1 x))
```

### Global Variables
```lisp
(defvar *xpto* 10)        ;; Does not overwrite old defvars
(defparameter *xpto* 10)  ;; Overwrites old values
(defconstant *xpto* 10)   ;; Setf on this is not defined
```

## Predicates - Functions that return booleans
```lisp
(zerop <x>)               ;; Checks if x is zero
(listp <x>)               ;; Is x a list? ;; NIL yields T
(atom <x>)                ;; Is x atomic (not a list)? ;; NIL yields T
(null <list>)             ;; Is list empty?
(not <x>)                 ;; Is x nil? (Negate x)
```

## Comparisons
```lisp
(= <n1> <n2>)             ;; Equal numbers, even of different types (different: /=)
(eq <e1> <e2>)            ;; Same object
(eql <e1> <e2>)           ;; eq or same number with same type, or char=
(equal <e1> <e2>)         ;; eql or lists with first and rest equal or arrays eq ; Isomorphism
(equalp <e1> <e2>)        ;; Deep equal, works with structures
(string= <s1> <s2>)       ;; String comparison
```

## Functions

### Defining functions
```lisp
(defun fact (n)
    "Returns the factorial of n."
    (if (zerop n)
        1
        (* n (fact (- n 1)))))

> (documentation fact)
"Returns the factorial of n."

;; Anonymous function:
(lambda (n)
    ...)
```
### Pass functions as arguments
```lisp
(dosomething #'fact)

(defun dosomething (f)
    (funcall f 10)                 ;; Calls f with inline arguments
    (apply f '(10)))               ;; Like funcall but receives list of arguments
```

### Multiple values
```lisp
;; Return multiple values:
(defun f ()
    (values 1 2))

(multiple-value-bind (x y)
    (f)
    ;; Here x and y are 1 and 2
    )
```

### Arguments
```lisp
(defun fn (a b &optional (c 10) &rest d &key e (f 30 f-supplied-p))    ;; f-supplied-p is T if f was given
     ...)

(fn 1 2)            ;; a = 1, b = 2, c = 10, e = nil, f = 30
(fn 1 2 3)          ;; c = 3, ...
(fn 1 2 3 4 5)      ;; ERROR: &key restricts what can be passed to &rest unless you add &allow-other-keys
(fn 1 2 :e 3)       ;; ERROR: LISP tries to match c with :e!!! /!\
```

### Local functions (let for functions)
```lisp
(flet ((<fun1> (<args>)                      ;; the equivalent to flet* is called labels
         ...)
       (<fun2> (<args2>)
         ...)
      )
      ;; Use fun1 and fun2 here
      )

(setf (fndefinition func) #'(lambda ...))                 ;; Redefine function. Doesn't work in Allegro
(symbol-function (find-symbol (string-upcase "print")))   ;; Works in Allegro
```

## Conditionals and Loops

### If
```lisp
(if <condition>
    <then>
    <else>)

;; Similar to (if x y nil)
(when <condition>
    <then1>
    ...
    <thenn>)

;; Similar to (if x nil y)
(unless <condition>
    <else1>
    ...
    <elsen>)
```

### Switch case
```lisp
(cond (<cond1>
       <then1>)
      ...
      (<condn>
       <thenn>)
      (t
       <default then>))

(case k ((1 2) 'clause1)
        (3 'clause2)
        (nil 'no-keys-so-never-seen)
        ((nil) 'nilslot)
        ((:four #\v) 'clause4)
        ((t) 'tslot)
        (otherwise 'others))

(typecase x
    (float "a float")
    (string "a string")
    (null "a symbol, boolean false, or the empty list")
    (list "a list")
    (t (format nil "a(n) ~(~a~)" (type-of x))))
```

### Multiple instructions (avoid: it's not lispy)
```lisp
(progn
    <instr1>
    ;; ...
    <instrn>)
```

### Loops
```lisp
;; Cycles for n = 0 ... 4
(dotimes (n 5 [return-value])       ;; Optional return value after cycle is done
    ...)

;; Cycles for each element in list
(dolist (n list [return-value])
    ...)

(do ((<var1> <init1> <step1>)       ;; Do cycle with multiple variables
     ...
     (<varn> <initn> <stepn>))      ;; var2 can use var1 in increment, not in initialization
    (<test> <return-value>)
     ...
    )

(do*                                ;; Initializes in order and increments in order

;; Explicit return from any loop or block/function:
(return [val])

;; Infinite cycle until (return), has exhaustive syntax for different behaviors
(loop ...)
```

## Input / Output
```lisp
(print "Hello")                    ;; Prints Hello on screen
(format t "Hello ~a" x)            ;; Like printf. ~a is anything, ~% is newline, ~& is newline if not blank line already, ~~ is tilde
(format nil ...)                   ;; Return string (like sprintf)
(format <stream> ...)              ;; Print to stream (ex: file)

(read [<stream> <eof-error-p> <eof-value>])   ;; Read from stream, optionally error on EOF or instead return a specific value.
(read)                                        ;; Read from stdin
(read-line ...)                               ;; Read whole line
(read-from-string <string> ...)               ;; Read from string

(with-open-file (file "filename.txt" :direction (:input|:output)
                 :if-exists (:overwrite|:append) :if-does-not-exist (nil|:error|:create))
    ;; ...
    (read-line file ...)
    (read-char file ...)
    (read-char-no-hang file ...))
```

## Packages
```lisp
*package*                    ;; Current package
(make-package 'p1)           ;; Creates package
(in-package p1)              ;; Start using package p1
(export x)                   ;; Makes symbol x public
p1:x                         ;; Accesses exported symbol
(import 'p1:x)               ;; Allows references to x instead of p1:x
(use-package p1)             ;; Imports all exported symbols
p1::x                        ;; Accesses symbol regardless of export
(shadowing-import 'pi:x)     ;; Import and overwrite existing imports

(def-package <name>
             (:use <package> ...)
             (:import-from <package> <symbol> ...)
             (:export <symbol>...))

(intern ...)                 ;; Manually import symbol
(unintern ...)               ;; Manually cancel symbol import
```

## Generated Symbols
```lisp
(make-symbol <name>)         ;; Creates symbol called name
(symbol-name 'symbol)        ;; Returns name of symbol as string
(gensym)                     ;; Creates new symbol with unique name
(gensym "prefix")            ;; Creates new symbol with prefix
(gensym x)                   ;; Creates new symbol with number x
```

## Macros
```lisp
(defmacro <name> (arg1 arg2)               ;; Backtick ` forbids evaluation except of expressions with , before
     `(dosomething-with ,arg1 ,arg2))

(defmacro avg (&rest args)                 ;; @args to pull them out of list
    `(/ (+ ,@args) ,(length args)))

(defmacro for (var start stop &body bd)    ;; When aux vars are used, gensym is required to avoid overwriting vars with same name
    (let ((gstop (gensym)))                                       
        `(do ((,var ,start (1+ ,var))                                
              (,gstop ,stop))
             ((>= ,var ,gstop))
             ,@ bd)))
```

## Macro Characters
```lisp
(defun dollars (x)                         ;; Example function to interpret dollars
    (* x 1.0))
(set-macro-character #\$ #'|$-reader|)     ;; #$100 is interpreted by |$-reader|
(defun |$-reader| (stream char)            ;; |$-reader| recursively reads stream and applies the dollars function to arguments
    (list 'dollars (read stream T nil T)))

(set-dispatch-macro-character ...)         ;; Creates family of macro characters with optional arguments: #1.1$20
```                                                                               

## CLOS - Common Lisp Object System

### Definition
```lisp
(defclass <name> (<superclasses>)
    ((<attr1> :initarg :<attr1> :initform <default> :accessor <attr1>)
     ...)
    (:documentation "This class represents this and that."))

(make-instance 'class)
(make-instance 'class :attr1 10)
```

### Types
```lisp
(type-of <instance>)                       ;; Returns type of instance
(typep <instance> <type>)                  ;; Whether instance is of type t
(subtypep <instance> <type>)               ;; Whether instance is subclass of t
```

### Class methods
```lisp
(defgeneric <name> (a1 a2)
    (:documentation "Does xyz"))

(defmethod <name> ((a1 <class>) a2) ...)           ;; Specializes method to a class
(defmethod <name> :before ((a1 <class>) a2) ...)   ;; Method to be called before the other specializations
(defmethod <name> :after ((a1 <class>) a2) ...)    ;; Method to be called before the other specializations

(defmethod <name> :around ((a1 <class>) a2)        ;; Method to be called around the other specializations.
    ;; ...
    (let ((res (call-next-method)))            
         ;; ...                                    
         res))                                     ;; May return a different value than the inner method

(defmethod <name> (a1 (a2 (eql :keyword))) ...)    ;; Specializes a method to a keyword
```

Order of methods' execution:
1. Specific around -> generic arounds
2. Specific befores -> generic befores
3. Most specific Primary method
4. Generic afters -> specific afters

Primary method disambiguation happens by choosing the most specific one. The leftmost most specific specialization wins. Only one primary method is called.

Calling superclass methods in specialization: `(call-next-method)` as well.

### "Static" (as in java static) attributes
```lisp
(<attr1> :initarg :<attr1> :initform <default> :accessor <attr1> :allocation :class)
```

### Extend accessors, constructor or print
```lisp
(defmethod (setf <accessor>) :after (<newval> (c <class>)) ...)
(defmethod <acessor> ...)
(defmethod initialize-instance :after ((c <class>) &key) ...)
(defmethod print-object ((c <class>) stream) ...)
```

### LISP types / constants specialization
```lisp
(defmethod divide ((n number) (m number))
    (/ n m))

(defmethod divide ((n number) (m (eql 0)))        ;; Must have eql predicate
    (error "Division by 0"))
```

## Optimization
```lisp
;; Instruct the compiler globally. 3 means agressive optimization 
(proclaim (optimize (size <0-3>) (speed <0-3>) (safety <0-3>) (debug <0-3>)))

;; Ignore unused argument locally
(declare (ignore <parameter>))

;; Show generated bytecode
(disassemble 'func)
```

To avoid stack overflows, use Tail-Recursion: add an aux parameter to the function that describes the current "res", the value to return in case of the stop condition. E.g.: `(* x (fact (- x 1)))`  -->  `(fact (- x 1) (\* x aux))`

## Debugging
```lisp
(ide:trace-format ...)             ;; "Printf"
(trace <function>)                 ;; Display calls and return values of function
(trace ((flet <outer> <inner>)))   ;; Trace flet-defined function
(untrace <function>)               ;; Cancel trace
(break)                            ;; Breakpoint here
```

## Conditions (Exceptions)
```lisp
(handler-case                        ;; High-level, control jumps back from failure
    (do-something)
    (my-error-condition (e)
        (print "Error found")))

(handler-bind ((my-error-condition   ;; Lower-level, control stays inside failure
    #'(lambda (e)
        (print "Error found!"))))
    (do-something)
    (do-something-else))             ;; Supports multiple instructions without progn
```
