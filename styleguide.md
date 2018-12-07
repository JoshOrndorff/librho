Rholang Style Guide
====================

This style guide is intended to provide consistency in naming and formatting in librho's rholang. Rholang is young so best practices and the language itself will change. Consider this a first attempt.

Formatting
---------------
### Indentation
Indent code blocks surrounded with `{ }` by two spaces.

Correct:
```scala
new chan in {
  chan!("Hello")
}
```

Incorrect:
```scala
new chan in {
chan!("Hello")
}

new chan in {
    chan!("Hello")
}
```

### Par Placement
The `|` symbol should be placed on the final (often only) line of the process after a single space.

Correct:
```scala
myChan!(myProc) |

for (_ <- myChan) {
  // Do something
} |
```

Incorrect
```scala
| myChan!(myProc)
|

for (_ <- myChan) {
  // Do something
}
|
```

### New Channels
When creating multiple unforgeable names, create the ones that will be attached to the system powerbox first. Names should be declared on a single line, or one per line.

Correct:
```scala
new x, y, x in { ... }

new stdout(`rho:io:stdout`),
    timestamp(`rho:block:timestamp`),
    contract,
    dataCh
    in { ... }
```

Incorrect:
```scala
new x, y, stdout(`rho:io:stdout`),
    dataCh
    in { ... }
```

Naming
---------------

### Filenames
Files ending with `.rho` extensions are expected to be valid rholang code.

Files that are largely rholang but require preprocessing should add a further extension such as `mytemplate.rho.pre`

### Powerbox Channels
When creating channels connected to the system powerbox, the channel should be named the same as the last portion of the powerbox name.

Correct:
```scala
new stdout(`rho:io:stdout`),
    timestamp(`rho:block:timestamp`),
    in { ... }
```

Incorrect:
```scala
new print(`rho:io:stdout`),
    time(`rho:block:timestamp`),
    in { ... }
```

### Process Variable Binding
Use and `@` sign in a for comprehension when you only intend to use the process ad data, and nothing will be sent over it.

Correct:
```scala
contract double(@n, return) = {
  return!(2 * n)
}
```

Incorrect:
```scala
contract double(n, @return) = {
  @return!(2 * *n)
}
```

### Case Convention
Names and processes are written in lowerCamelCase.

Correct:
```scala
new favNumCh in {
  favNumCh!(5) |
  for (favNum <- favNumCh) { ... }
}
```

Incorrect:
```scala
new favnumch in {
  favnumch!(5) |
  for (Favnum <- favnumch) { ... }
}
```

_The jury is out on this one for me. I see value in distinguishing names with UpperCamelCase_



Coding
-------------------

### Acknowledgments
When sending a message that serves only as an acknowledgement and carries no additional information, send `Nil`. The channel over which the acknowledgment is send could be called `ack`.

Correct:
```scala
contract printDouble(@n, ack) = {
  stdout!(2 * n) |
  ack!(Nil)
```

Incorrect:
```scala
contract printDouble(@n, return) = {
  stdout!(2 * n) |
  return!(6)
```
### Multiplexing
Contracts that serve as call and return functions take one channel for successful results, called `return`, and one for error messages and diagnostics called `error`.

Correct:
```scala
contract isPositive(@n, return, error) = {
  match n {
    Int => { /* Send answer on return */ }
    _ => error!("Only integers allowed in isPositive")
  }
}
```

Incorrect:
```scala
contract isPositive(@n, return) = {
  match n {
    Int => { /* Send ["Success", answer] on return */ }
    _ => return!(["Error", "Only integers allowed in isPositive"])
  }
}
```

Motivation:
* Either pattern can be converted to the other through multiplexing and demultiplexing. Muxing is cheap whereas demuxing costs a pattern match.
* This pattern allows errors in sub calls to be reported directly to the upstream caller without having to return through the call stack.
* This idea can be generalized to implement one-of types like `Maybe` which would take channels for `nothing`, `just`, and `error`.
