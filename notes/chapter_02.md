## Chapter 2: Do it, do it again, and again, and again...

### Concepts

* `(lat? l)`: says if a given list `l` doesn't contain any list.

```
(define lat?
	(lambda (l)
		((null? l) #t)
		((atom? (car l)) (lat? (cdr l)))
		(else #f)))
```

* `(member? a lat)`: says if the atom `a` is a member of the list of atoms `lat`.

```
(define (member? a lat)
	(lambda (a lat)
		(cond
			((null? lat) #f)
			(else (or (eq? (car lat) a) (member? a (cdr lat)))))))
```

### The First Commandment

*Always ask `null?` as the first question in expressing any function*.