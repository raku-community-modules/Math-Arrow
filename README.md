[![Actions Status](https://github.com/raku-community-modules/Math-Arrow/actions/workflows/linux.yml/badge.svg)](https://github.com/raku-community-modules/Math-Arrow/actions) [![Actions Status](https://github.com/raku-community-modules/Math-Arrow/actions/workflows/macos.yml/badge.svg)](https://github.com/raku-community-modules/Math-Arrow/actions) [![Actions Status](https://github.com/raku-community-modules/Math-Arrow/actions/workflows/windows.yml/badge.svg)](https://github.com/raku-community-modules/Math-Arrow/actions)

NAME
====

Math::Arrow - Handle Knuth-style arrow notation

SYNOPSIS
========

In Knuth arrow notation, one arrow is simple exponentiation:

```raku
2 ↑ 3 # gives 8
```

Every additional arrow in the operator implies a layer of reduction of the form:

```raku
a ↑{n number of ↑'s} b =
  a ↑{n-1 ↑'s} ( a ↑{n-1 ↑'s (... a)...)
```

where there are `b` number of `a`s in the second form.

But this module only implements up to five arrows (really, if you need more than five, I question what the hell you think Raku is going to do for you...)

More concretely:

```raku
4 ↑↑↑↑ 3 =
4 ↑↑↑ ( 4 ↑↑↑ 4 ) =
4 ↑↑↑ ( 4 ↑↑ ( 4 ↑↑ ( 4 ↑↑ 4 ))) =
4 ↑↑↑ ( 4 ↑↑ ( 4 ↑↑ ( 4 ↑ (4 ↑ (4 ↑ 4 ))))) =
4 ↑↑↑ ( 4 ↑↑ ( 4 ↑↑ ( 4 ** 4 ** 4 ** 4))) = ...
```

As you can see, this quickly gets out of hand. In fact, there are very few arrow-notation expressions which can be evaluated in a practical period of time (the above example would take longer to fully evaluate than the universe has been around, even on the fastest computers available, today.

Probably the best-known arrow-notation example is Graham's Number (G) which is defined as:

```raku
$g1 = 3 ↑↑↑↑ 3;
$g2 = arrow(3, 3, $g1);
...
$g64 = arrow(3, 3, $g63);
use constant G = $g64;
```

operator ↑
----------

The `↑` operator is an infix operator which is equivalent to `arrow(left-hand-side, right-hand-side, 1)` and `**` and performs simple exponentiation. It is right-associative, just like `**`.

operator ↑↑
-----------

The `↑↑` operator is also a right-associative infix operator and is equivalent to `arrow(lhs, rhs, 2)`.

operator ↑↑↑
------------

See `↑↑` for details. Equivalent to `arrow(lhs, rhs, 3)`.

operator ↑↑↑↑
-------------

See `↑↑` for details. Equivalent to `arrow(lhs, rhs, 4)`.

operator ↑↑↑↑↑
--------------

See <↑↑> for details. Equivalent to `arrow(lhs, rhs, 5)`.

arrow($a, $b, $arrows)
----------------------

Returns the arrow-notation expansion for `$a ↑[$arrows] $b` where `↑[$arrows]` denotes `$arrows` many `↑` in the final operator. Thus these two are equivalent:

```raku
arrow(3, 3, 4) == (3 ↑↑↑↑ 3)
```

term G
------

```raku
use Math::Arrow :constants;
say G;  # you won't have enough CPU or memory to see it
```

Only exported via the `:constants` tag.

The value `G` is defined as a more-or-less humorous element of this module. It can never be used for anything practical, as evaluating its value would take such a mind-bogglingly large amount of time and memory that you might as well simply shoot your computer. But... don't do that.

This value is Graham's number, defined as described above.

AUTHOR
======

Aaron Sherman

COPYRIGHT AND LICENSE
=====================

Copyright 2016-2017 Aaron Sherman

Copyright 2024 Raku Community

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.

