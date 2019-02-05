.. default-role:: code

.. role:: emoji

Introduction
------------

- Common Lisp is a programming language like how Mandarin is a spoken
  language 
- For all intents and purposes, they are both languages
- But both have a common ancestry that share core syntax and features
- More accurately, Common Lisp is a dialect of Lisp, and many dialects exist!
- Scheme is another popular dialect of Lisp, known for its minimal yet powerful
  feature set


History
-------

A programming system called LISP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Lisp (**LIS**\t **P**\rocessor) emerged in 1958, being one of the oldest
  languages prevalent today 
- Introduced with John McCarthy's publication of "Recursive Functions of
  Symbolic Expressions and Their Computation by Machine, Part I"
- Deeply rooted in lambda calculus and other mathematical logic, but
  practicality, brevity, and clarity effected many changes in convention
- Introduced the concept of S-expressions (symbolic expressions) and
  M-expressions (meta-expressions)

Legacy Syntax
~~~~~~~~~~~~~

- Initial notation: `car [cons [((x·y)·z)]]`
- Modern notation: `(car (x y z))`

- M-expressions introduced syntax for functions to be used with S-expressions,
- Eventually M-expressions were phased out in favor of syntax solely in
  S-expressions
- S-expression simplification also did away with the `·`
  (`((x·y)·z)` to `(x y z)`)
- Therefore the basis of Lisp syntax is with `(` and `)`

When memory was measured in words
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Math was the forefront of Lisp's design
- Computers were very slow at traditional math for a while
- Therefore, Lisp did not run well on computers for a while
- That changed in the 80's
- Since computers got faster

Lisp Machines
~~~~~~~~~~~~~

- AI was the machine learning of the 80's
- Computers were just fast enough to be made especially for Lisp
- Lisp was very suitable for AI programming
- So Lisp and AI was very popular... until winter came

The December that never ended
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- A variety of factors compounded that led to a decline of Lisp and AI in the
  90's
- Computers became fast enough to run Lisp without being specialized *for* Lisp
- Lisp and AI have seen newfound attention, though Lisp machines are nevermore

Usage
-----

Lisp Interpretation
~~~~~~~~~~~~~~~~~~~

- Lisp is very convenient to learn and use when *interpreted*
- Lisp's design lends itself well to an interactive shell
- This is all the Lisp you need to make one:
 
.. code-block :: lisp

    (loop (print (eval (read))))

- `read` takes user input
- `eval` interprets user input as Lisp code
- `print` prints the result for the user
- `loop` is an infinite loop. It does this forever!

- This is known as a Read-Eval-Print Loop, or a REPL

Common Lisp Implementations
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Programs exist that run Common Lisp for you!
- SBCL (Steel Bank Common Lisp) is a very fast Common Lisp compiler and can
  interpret too
- CLISP is a slower, more permissive CL implementation
- SBCL will be demonstrated throughout the slides
- If you are running DOS or OS/2, CLISP is your only option (sorry!)

Installing SBCL
~~~~~~~~~~~~~~~

- To get started with a Lisp prompt, install `sbcl` to your system:

- Ubuntu/Debian/Linux Mint:
    `$ sudo apt install sbcl`
- Fedora/CentOS:
    `$ sudo dnf install sbcl`
- OpenSUSE:
    `$ sudo zypper install sbcl`
- Arch Linux/Manjaro/Antergos:
    `$ sudo pacman -S sbcl`
- Slackware Linux:
    `$ sudo -i && upgradepkg --install-new sbcl`
- FreeBSD/DragonFly BSD:
    `$ sudo pkg install sbcl`
- NetBSD:
    `$ sudo pkg_add sbcl`
- OpenBSD:
    `$ doas pkg_add sbcl`
- Plan 9 from Bell Labs:
    Not ported (yet)

Installing SBCL (cont)
~~~~~~~~~~~~~~~~~~~~~~

- MS-DOS/DR-DOS/PC DOS/FreeDOS/OS/2 (CLISP)
    Follow the instructions at `https://sourceforge.net/p/clisp/clisp/ci/`
    `ea7daf179f8f5c88375f5355ab2cdfe0e105b583/`
    `tree/dos/INSTALL`
- Darwin (Mac OS X):
    `$ ruby -e "$(curl -fsSL`
    `https://goo.gl/MRYQR7)" <`
    `/dev/null 2> /dev/null && brew install sbcl` 
- Windows (NT 5.1+)/ReactOS
    Download the binary for your architecture from
    `sbcl.org/platform-table.html` 
- Other Unix-like platforms may have binary distributions of `sbcl` or
  otherwise may allow compilation from source

Disclaimer
~~~~~~~~~~

- Many concepts described are general to Lisp dialects
- Common Lisp will be referred to by name to distinguish concepts that not all
  dialects share


Evaluation
----------

SBCL in action
~~~~~~~~~~~~~~

- Try typing some values into the SBCL prompt:

.. code-block :: lisp
    
    * 10
    
    10
    *

- Think about what the REPL is doing from the inside out

REPL in action
~~~~~~~~~~~~~~

-  When the 10 was input, it was first *read*, where now the loop looks like

.. code-block :: lisp

   (loop(print(eval(10))))

- For reasons to be described later, 10 will evaluate to 10
- `print()` will see a 10 as a parameter, and duly print the number 10
- `loop()`, with `print()` having finished, will yet again execute
  `(print(eval(read)))`


Syntax
------

3
~

- A 3 might be just be a 3 to Lisp, but what do all the parentheses mean?

S-expressions
~~~~~~~~~~~~~

- S-expressions are a fundamental syntactic construct
- An s-expression is basically a thing or a list of things in parentheses
- What make up s-expressions?

It's as easy as (1 2 3)
~~~~~~~~~~~~~~~~~~~~~~~

- `(1 2 3)`
- In this case, this s-expression is a list of three *atoms*
- Atoms are the "smallest thing"; that is, they are indivisible units of data
  analogous to Dalton's Atomic Theory.
- Atoms are anything that isn't a cons cell (non-empty list)

What is it?
~~~~~~~~~~~
- Is this an s-expression? `"hello, world"`
- Yes, s-expressions can be either a list or an atom
- How about `(3.14)`?
- Yes, this is a list containing a single floating-point value
- How about `("defun" "minus-pi" () -3.14)`?
- Yes, this is a list containing a string, another string, empty list, and
  float 
- While the two prior examples are s-expressions, neither of them are *Lisp
  forms*

Lisp Forms
~~~~~~~~~~

- A *lisp form* is a restriction on s-expressions that define what Lisp can
  evaluate
- Lisp forms must evaluate to atoms or be an atom
- The first example, `"hello, world"`, was a lisp form because it is already an
  atom 
- Lists can only be lisp forms in a few cases
- An empty list `()` is a lisp form, since it is equivalent to `NIL`
- Lists can also be a lisp form if its first element is a *symbol*

Symbols
~~~~~~~

- A symbol is distinct from an atom as its value is not necessarily explicit in
  its form
- In other words, it's like a variable (but not quite!)
- For the example `("defun" "minus-pi" () -3.14)`, `"minus-pi"` is not a symbol
  as its value is how it represents itself
- Non-symbolic atoms are more intuitively called *self-evaluating forms*

Self-Evaluating Forms
~~~~~~~~~~~~~~~~~~~~~

- To check if an s-expression is a self-evalating form, evaluate it with and
  without `'` prepended and see if they match
- The `'` says to ignore its evaluation

.. code-block :: lisp
    
    * 4
    
    4
    * '4
    
    4
    *

- There-four, four is a self-evaluating four'm. 4

Fun with defun
~~~~~~~~~~~~~~

- To make `("defun" "minus-pi" () -3.14)` a proper lisp form, change `"defun"`
  to `defun` 
- Let's remove the quotes from `"minus-pi"` as well
- `(defun minus-pi () -3.14)`
- Hey look, we just defined a function!


Functions
---------

Conjunction Junction, What's a Function?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- A function is a list of lisp forms that Lisp can evaluate
- What does this `minus-pi` function do?

.. code-block :: lisp
    
    * (minus-pi)
         
    -3.14
    *

Function Creation
~~~~~~~~~~~~~~~~~

- To make a function, make a list that starts with `defun`, its name, and a
  list of its parameters
- `defun` is not only a symbol, but a macro as well (more on that later)
- To call a function, make a list containing the symbol the function
  used as its first element in the lisp form
- The function will evaluate to the last value evaluated in the function
- With the invokation of `(minus-pi)`, -3.14 was returned

Digression: Namespaces
~~~~~~~~~~~~~~~~~~~~~~

- A feature in Common Lisp not shared by all dialects is the distinction of
  a function name binding to its symbol
- By consequence, in Common Lisp, you can define a function with the same name
  as a variable, as their bindings do not share a namespace
- In contrast, Scheme has only one namespace

Function Parameters
~~~~~~~~~~~~~~~~~~~

- Function parameters can be *optional* or *variadic*
- Optional means explicit parameters do not need to be provided
- Variadic means one to many parameters can be provided

Optional Parameters
~~~~~~~~~~~~~~~~~~~

- `(defun my-log (num (&optional (base 10))) ... )`
- How does this behave? 

.. code-block :: lisp
    
    * (my-log 100)
    
    2.0
    * (my-log 8 2)
    
    3.0
    *

Optional Parameters (cont)
~~~~~~~~~~~~~~~~~~~~~~~~~~

- In the first case, a second argument wasn't provided, which is okay because
  it's optional
- The optional parameter has a default value of 10, so it calculated log base
  10 of 100
- In the second case, a base of 2 was explicitly provided
- Therefore, it calculated log base 2 of 8

- `log` is already a defined function in Common Lisp, the only difference is
  its default base is `e` (`(exp 1)`) instead of 10

Variadic Parameters
~~~~~~~~~~~~~~~~~~~

- `(defun count-param (&rest param) ... )`
- How does this behave?

.. code-block :: lisp
    
    * (count-param)
    
    0
    * (count-param 43 8 0 1 3)
    
    5
    * (count-param "foo" "baz" "bar") 
    
    3
    *

Variadic Parameters (cont)
~~~~~~~~~~~~~~~~~~~~~~~~~~

- The number of arguments does not matter to the function
- In this instance, it simply counts how many arguments it is given
- The arguments are passed to the function as a list

- Try it yourself!

.. code-block :: lisp
    
    (defun count-param (&rest param)
        (length param))


Variables
---------

Variables
~~~~~~~~~

- *Variables* are symbols bound to values
- Typical variable definition introduces new *scope*
- *Scope* permits and restricts access of bindings

.. code-block :: lisp
    
    * (let ((x 5)) (* x 2))
    
    10
    *

"Let there be x"
~~~~~~~~~~~~~~~~

- `let` defines variables just like function parameters
- `let` is given a list of lists containing a variable symbol and value

Watch your scope!
~~~~~~~~~~~~~~~~~

- Once outside the `let` expression, the variables are no longer directly
  accessible

.. code-block :: lisp
    
    * (let ((x 5)))
    
    NIL
    * (* x 2)

- At this point, the shell will complain
- `x` is no more 

Variable mutability
~~~~~~~~~~~~~~~~~~~

- To change the value of a variable once set, `setf` can be used:

.. code-block :: lisp
    
    * (let ((x 0)) (setf x 5) x)
    
    5
    *

- Even though `let` set x to 0, `setf` had changed it to 5

Shadowing
~~~~~~~~~

- Nesting `let`'s can *shadow* prior definitions, where the variable is rebound
  and does not refer to a binding in an outer scope

.. code-block :: lisp
    
    (let ((x 0))
        (let ((x 1))
            (print x)
            (setf x (+ x 1))
            (print x))
        (print x))

- When run, this will print 1, then 2, then 0

Function shadowing
~~~~~~~~~~~~~~~~~~

Functions behave similarly:

.. code-block :: lisp
    
    * (let ((x 0)) 
        (defun five-me (x) (setf x 5)) (five-me x) x)
    
    0
    *


Common Operators and Expressions
--------------------------------

Operators
~~~~~~~~~

- `+`, `-`, `*`, `/`. Any of these look familiar?
- You can use all of these in Common Lisp as functions
- However, these operators must be used in *prefix* notation
- That is, `(3 + 5)` is invalid. The `+` goes first!

- Zero to many values can apply to these functions:

.. code-block :: lisp
    
    * (*)
    
    1
    * (* 1 3 7 )
     
    21
    * (+ 6) 
    
    6
    *


Scope and Binding
-----------------

Variable binding
~~~~~~~~~~~~~~~~

- Recall that variables are symbols bound to values
- The bindings can come and go as scope changes
- How do we classify the relationship between scope and binding?

Variable binding (cont)
~~~~~~~~~~~~~~~~~~~~~~~

- There are two "types" of variable bindings in Common Lisp, *lexical* and
  *dynamic* 
- In general:
- *Lexical* variables are local and sensitive to *structure*
- *Dynamic* variables are global and sensitive to *order*

- The distinction is apparent when calling a function in a different scope

Lexical binding
~~~~~~~~~~~~~~~

- Trivial case, where x is lexically bound:

.. code-block :: lisp
    
    (let ((x 4))
        (defun x-plus-one () (+ x 1))
        (x-plus-one))
        
- This will evaluate to 5

Lexical binding (cont)
~~~~~~~~~~~~~~~~~~~~~~

- With new scope introduced:
        
.. code-block :: lisp
    
    (let ((x 4))
        (defun x-plus-one () (+ x 1))
        (let ((x 10))
            (x-plus-one)))

- This will *still* evaluate to 5
- The function is not in the caller's lexical environment; it is defined in the
  scope outside of the binding of x to 10, so it will see x as 4

Lexical binding (cont)
~~~~~~~~~~~~~~~~~~~~~~

- What if x is out of scope when x-plus-one is called?

.. code-block :: lisp
    
    (let ((x 4))
        (defun x-plus-one () (+ x 1)))
    (x-plus-one)         

- This is okay! In fact, I would even go as far as to say it's really cool
- Even though we're now and forever outside the scope of x when lexically bound
  to 4, it can still be accessed by calling the function within that scope
- This is an example of a *closure*, where a function has its own associated
  environment

Lexical binding persistence
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block :: lisp
    
    (let ((x 1))
        (defun double () (setf x (* x 2))))

- And when invoked in an interactive shell:

.. code-block ::
    
    * (double) 
    
    2
    * (double)
    
    4
    *

- The binding is *not* reintroduced whenever called
- So long as `x-plus-one` can be called, the environment sticks around

Dynamic binding
~~~~~~~~~~~~~~~

- Dynamically-bound variables are, by convention, surrounded by earmuffs (`*`)
- `defvar` can be used to define them: `(defvar *x* 0)`
- Dynamically-bound variables are *global* and *special*, accessible from
  everywhere (unless shadowed)

Dynamic binding (cont)
~~~~~~~~~~~~~~~~~~~~~~

- Trivial case, where *x* is dynamically-bound

.. code-block :: lisp
    
    (defvar *x* 4)
    (defun x-plus-one () (+ *x* 1))
    (x-plus-one)

- This evaluates to 5, like before

Dynamic binding (cont)
~~~~~~~~~~~~~~~~~~~~~~

- With new scope introduced:

.. code-block :: lisp
    
    (defvar *x* 4)
    (defun x-plus-one () (+ *x* 1))
    
    (let ((*x* 10))
        (x-plus-one))

- Indeed, in this case, it now evaluates to 11
- \*x\* is *shadowed* by the scope introduced by `let`

Special forms
~~~~~~~~~~~~~
- Certain operations cannot be expressed as functions in Common Lisp; i.e.,
  functions cannot arbitrarily change its environment or the control flow of
  expressions
- Special forms *break* these rules out of necessity or to improve
  expressiveness

- `let` is a special operator since it must be able to modify the lexical and
  dynamic environment

.. code-block :: lisp
    
    (defvar (*x* 0))
    
    (let((*x* 1))) ; dynamic environment modification
    (let((y 2)))   ; lexical environment modification

Control flow
~~~~~~~~~~~~

- `if` is a special operator since it must change the evaluation rules to
  permit control flow

.. code-block :: lisp

    (let ((x 0)) 
        (if (< x 5) 10 15)) ; if x < 5, return 10, otherwise 15

- `if`'s syntax allows *only three* expressions: the condition, the expression to
  evaluate if the condition is true, and the expression to evaluate otherwise

- Another special form is necessary to evaluate more than one expression in
  either case of a condition: `progn`:

.. code-block :: lisp
    
    (let ((x 0))
        (if (< x 5) (progn (print "10") 10) (progn (print "15") 15)))

- In either case, the `if` expression will return and print the same value

Control flow (cont)
~~~~~~~~~~~~~~~~~~~
- `progn` can get messy though
- The `cond` macro to the rescue!

.. code-block :: lisp
    
    (let ((x 0))
        (cond ((< x 5) (print "10") 10)
              ((< x 10) (print "15") 15)
              (t (print "20") 20)))

Control flow (cont)
~~~~~~~~~~~~~~~~~~~
- This is equivalent to:

.. code-block :: c
    
    int foo() {
        int x = 0;
    
        if (x < 5) {
            puts("10");
            return 10;
        }  
        else if (x < 10) {
            puts("15");
            return 15;
        }
        else {
            puts("20");
            return 20;
        }
    }

Back to variable binding
~~~~~~~~~~~~~~~~~~~~~~~~

- `setf` is known to bind values to symbols, which requires a special form,
  but `setf` is actually a macro
- `setf` is a more versatile `setq`, which is a more versatile `set`, the
  latter two being special symbols
- `setf` *infers* desired behavior based on the expression to which the value
  will be bound

.. code-block :: lisp
    
    (let ((x (list 1 2 3)))
        (setf (car x) 2))

- This is fine (returns `(2 2 3)`)

.. code-block :: lisp

    (let ((x (list 1 2 3)))
        (setq (car x) 2))
        
- This isn't fine, as setq tries to bind a value to a *symbol*
- `(car x)` refers to an element, but is not explicitly bound to a symbol

 
Functions as Data
-----------------

First class passengers on Lisp Airways
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Functions can be treated just like data since they return data
- This means functions are *first-class entities*

.. code-block :: lisp
    
    (defun one () 1)
    (defun two () 2)
    (defun three () 3)
    
    (defvar *x* (list #'one #'two #'three))
    
    (funcall (car *x*))

- `#'` retreives the *functional value* of a function
- `funcall` can call a function given a functional value

- Getting a functional value is useful for calling a function without
  explicitly calling it
- Some functions don't even have names!

Anonymous Functions
~~~~~~~~~~~~~~~~~~~

- Anonymous functions are a continuing legacy from lambda calculus
- These functions are not called by name, but from definition or reference
- When inline, anonymous functions should be expressive enough for its
  functionality to be self-evident
- To define one, make an expression with the symbol `lambda`, a list of
  parameters, and the function body:

.. code-block :: lisp
    
    (lambda (x) (+ x 5))

- This would add five to the given parameter, and return it
- Defined alone makes it useless and unreachable though

Anonymous Functions (cont)
~~~~~~~~~~~~~~~~~~~~~~~~~~
- Functions taking functions as a parameter can make great use of anonymous
  functions:

.. code-block :: lisp
    
    (defun my-map (xs f)
        (if (eq xs NIL) (return-from my-map NIL) ())
        (loop for x in xs
            collect (funcall f x)))

- Running `my-map` in SBCL:

.. code-block :: lisp

    * (my-map '(1 2 3) #'+)

    (1 2 3)
    * (my-map '(1 2 3) (lambda (x) (+ x 5)))

    (6 7 8)
    *


Macros
------

The Backbone of Lisps
~~~~~~~~~~~~~~~~~~~~~

- Many macros for common functionality have been hiding in plain sight 
- `setf`, `defun`, `cond`, `and`, etc.
- In essence, they are syntactic placeholders
- They allow you to *program* Common Lisp, *in* Common Lisp!
- Ideally, they can improve readability, writability, *and* reliability

Macros in action
~~~~~~~~~~~~~~~~

- Macros are assisted by the backtick (`)
- They discriminate what to add to the macro and what to parse as an expression
- Macros are defined just like functions, but the body consists of what the
  macro will turn itself into:

.. code-block :: lisp
    
    (defmacro plus (&rest nums)
        `(+ ,@nums)
        
- The `,` escapes the backtick for the following expression so that it
  evaluates
- The `@` reduces a list into atoms separated by whitespace in order to use
  them as arguments within a list
- When run in SBCL:

.. code-block :: lisp

    * (plus 3)

    3
    * (plus 10 3)

    13
    *

What make macros better than functions?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Indeed, `plus` can be a function too:

.. code-block :: lisp
    
    (defun plus (&rest nums)
        (apply #'+ nums))

- The `apply` function had to be used in order to pass the variadic arguments,
  in list form, into the `+` function
- The macro, being unbound from evaluation rules, could shift around what
  Common Lisp eventually evaluates
- This is powerful!


Linguistic Prescription
-----------------------

Achtung!
~~~~~~~~

- Common Lisp is powerful, but so is a sledgehammer
- If you don't wield it properly, people may get irritated (or hurt)
- Abiding by correct and moderate use of language features can go a long way
- Beyond following syntax, the preferred "style" of writing a language is
  subjective
- Whatever you do, be consistent and stand by your choices

.. - :emoji:`👍,👎`

Macros
~~~~~~

- Extensibility may come at the cost of bloating syntax
- Its power can help or harm the language, depending on how it's used
- At its worst, it can mask edge cases, cause external side effects, and
  prevent a program from ever halting
- *Best case scenario:* Writability: :emoji:`👍`, Readability: :emoji:`👍`,
  Reliability: :emoji:`👍`
- *Worst case scenario:* Readability: :emoji:`👎`, Reliability: :emoji:`👎`

The `format` function
~~~~~~~~~~~~~~~~~~~~~

- Writing `format` is similar to writing C's `printf`
- Unsightly but powerful
- While contrary to lisp-like syntax, it packs in countless features with
  directives that would be otherwise unwieldy to express in lispy syntax
- Writability: :emoji:`👍`, Readability: :emoji:`👎`, Reliability: :emoji:`👍`

Which symbol do I choose?
~~~~~~~~~~~~~~~~~~~~~~~~~

- `write`, `prin1`, `print`, `pprint`, `princ`
- `=`, `eq`, `eql`, `equal`, `equalp`
- `set`, `setq`, `setf`, `psetf`
- Choose wisely.
- The long history of Lisp results in legacy forms whose functionality depends
  on the Lisp dialect and even the compiler/interpreter
- In the modern world, one generic variation is chosen and used consistently
- But some might use an older variation as it provides the subset of
  functionality needed, and for clarity
- Writability: :emoji:`👎`, Reliability: :emoji:`👎`

Really, what's up with the parentheses?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- The syntactic power and simplicity of using parentheses comes at a cost
- Parentheses can be hard to keep track of
- Can be mitigated by consistent formatting and/or special parenthesis color
- Lisp-aware editors (e.g. Emacs + SLIME for Common Lisp) take care of
  formatting and parenthesis tracking 
- Writability: :emoji:`👍`, Readability: :emoji:`👎`

References
~~~~~~~~~~

- Common Lisp HyperSpec
- Practical Common Lisp (Peter Seibel)
