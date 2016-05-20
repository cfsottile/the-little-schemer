## Chapter 7: Friends and Relations

### Concepts

* **set**: list of non-repeated atoms.
* **pair**: list of two S-expressions.
* **rel**: set of pairs.
* **fun**: rel where `firsts` form a set.
* **fullfun**: fun where `revrel fun` is also a fun, or, in other words, fun where `seconds rel` is a set.

### Operations

##### `fun? rel`

**Input data**: a relation.

**What to do**: check if `rel` is a function. In order to be a function, `firsts rel` must be a set.

```
(define fun?
	(lambda (rel)
		(set? (firsts rel))))
```

##### `revrel rel`

**Input data**: a relation.

**What to do**: build a new rel where `firsts newrel` are `seconds rel` and `seconds newrel` are `firsts rel`.

**What's the *current* element**: `car rel`.

**What's the natural recursion**: `cdr rel`.

**What can happen**:

* `null? rel`: return '()
* else: `(cons (build (second (car rel)) (first (car rel))) (revrel (cdr rel)))`
	* if we had a `revpair pair` function, we could state: `(cons (revpair (car rel)) (revrel (cdr rel)))`

##### `fullfun? fun`

**Input data**: a function.

**What to do**: say the truth about `seconds fun` being a set, or about `revrel fun` being also a a fun.

```
(define fullfun?
	(lambda (fun)
		(set? (seconds fun))))
```

```
(define fullfun?
	(lambda (fun)
		(fun? (revrel fun))))
```
