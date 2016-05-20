## Chapter 1: Toys

### Concepts

* **atom**: any literal
* **list**: any `()` with S-expressions inside
* **S-expression**: any atom or list - *or expression?*
* `(car l)`: first S-expression of `l`.
	* `l` must be a non-empty list.
* `(cdr l)`: `l` without its first S-expression.
	* `l` must be a non-empty list.
	* The result is *always* a list.
* `(cons s l)`: adds the S-expression `s` at the beginning of the list `l`.
* `(null? l)`: `true` if `l` is an empty list, `false` otherwise.
	* `l` *should* be a list.
* `(atom? s)`: `true` if the S-expression `s` is an atom.
* `(eq? a1 a2)`: `true` if `a1` and `a2` are both the same non-numeric atom, `false` if they're different non-numeric atoms.
	* `a1` and `a2` *should* be atoms. In practice, they may be lists.
	* `a1` and `a2` *should* be non-numeric atoms. In practice, they may be some numeric atoms.

### Laws

* **The Law of Car**
	* The primitive `car` is defined *only* for non-empty lists.
* **The Law of Cdr**
	* The primitive `cdr` is defined *only* for non-empty lists.
	* The `cdr` of any non-empty list is *always* another list.
* **The Law of Cons**
	* The primitive `cons` takes two arguments.
	* The second argument *must* be a list.
	* The result is a list.
* **The Law of Null?**
	* The primitive `null?` is defined *only* for lists.
* **The Law of Eq?**
	* The primitive `eq?` takes two arguments.
	* Each *must* be a non-numeric atom.