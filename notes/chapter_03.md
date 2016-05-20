## Chapter 3: Cons the Magnificent

### The Second Commandment

*Use `cons` to build lists*

### Concepts

##### `rember a lat`

Stands for _**rem**ove a mem**ber**_. To do so, it constructs a new list which contains every atom in `lat` but the first ocurrence of `a`.

*How does it work?* A priori, we can say that:

* it checks if the first element of `lat` is `eq?` to `a`
	* if it is, it returns the `cdr` of `lat`, since it is what we want
	* if it is not, it recurs with `lat` being the `cdr` of `lat`: `(rember a (cdr lat))`; since `a` should be a member of the new list, it doesn't *just* recur, it also `cons` the `car` of `lat`, because the recursion *will* give us `lat` without the first ocurrence of `a`

As you can see, we are *not* following **The First Commandment**. It tells us that the first thing we should do is asking `null?`. In our case, we need to do it because if `lat` does not contain `a`, the recursion will reach the empty list, and we must be prepared for that case.

Yeah, ok, but.. what shall `rember` do when `lat` is `null`? Well, if we are getting this value, it is because some of these situations occurred:

* the first application's `lat` did not contain `a`
* this is the first application, so `lat` is originally `null`

If `lat` didn't have `a` as a member, all the others applications of the function are waiting our response to `(cons (car lat) ...)`. If `lat` was the first 5 naturals, it would be something like:

```
(cons 1
	(cons 2
		(cons 3
			(cons 4
				(cons 5
					...
```

I have to return an `()` for the list to *start* building, or I have to return an `()` because that is the expected result when `lat` is an empty list. I don't really care which one is occurring.

The code:

```
(define (rember a lat)
	(lambda (a lat)
		(cond
			((null? lat) '())
			(else
				(cond
					((eq? (car lat) a) (cdr lat))
					(else
						(cons (car lat) (rember a (cdr lat)))))))))
```

##### Why `((null? l) '())`

For example, at `rember a lat`. At each step of the recursion, we are taking care of *one* element. We compare that element to another. If it's equal, we build a list and return it. But if it isn't, we will recur, *adding* the element onto the resulting list of the recursion. If we are in the last recursive step, we will `cons` the last element of `lat` onto the result of `rember a '()`. Since `cons` expects a list as the second argument, we would be great returning `'()` when `lat` is `null`.