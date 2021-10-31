# marina
Yet another SAT solver written in Ocaml, `marina` being the Malagasy word for `True`.

## Practical example

_Marina ve fa misy ny zazavavindrano?_
Is it true that Zazavavindrano exist?
The following assumptions were gathered across the Malagasy folklore:
* Zazavavindrano who don't swim in warm sea have red stripes
* Zazavavindrano either have blue stripes or does not have red stripes
* Zazavavindrano who live in the corals don't eat shrimps
* A zazavavindrano eats shrimps if and only if she swims in warm sea
* Zazavavindrano who have blue stripes swim in warm sea and live in the corals
* Zazavavindrano who swim in warm sea have blue stripes

```
$ ./marina '(~ swim_warm -> red)
          & (blue | ~ red)
          & (live_corals -> ~ eat_shrimps)
          & (eat_shrimps <-> swim_warm)
          & (blue -> (swim_warm & live_corals))
          & (swim_warm -> blue)'
> (,false)
```

## Usage
```
$ brew install ocaml
$ make
$ ./marina '(a&b | c)->d  <->  ~e'
(a,true) (b,true) (d,true) (e,false)
```

Notice that provided assignement is _partial_:
value of `c` has no effect on the satisfiability of the whole proposition.

General definition of a proposition `<prop>` is as follows:
```
<prop> ::=  T
          | F
          | <atom>
          | ~ <prop>
          | <prop> & <prop>
          | <prop> | <prop>
          | <prop> -> <prop>
          | <prop> <-> <prop>
          | (<prop>)
<atom> ::= ^[a-z_]+[0-9]*$
```

## Acknowledgment

Underlying decision procedure is based on IF-expression normalization:
https://www.lri.fr/~filliatr/ftp/tp-info/IFexpressions.pdf