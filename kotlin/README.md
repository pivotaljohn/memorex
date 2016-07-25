

# Raw Running Notes

* various operators are "merely" translated by the compiler into method calls. (see also https://kotlinlang.org/docs/reference/operator-overloading.html)
  * `in` => `.contains()`
  * `in!` => `!.contains()`
