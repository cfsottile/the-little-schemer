## `rember* a l`

Voy a construir una lista. Recibo una lista `l`, y quiero armar una nueva con los mismos elementos, exceptuando aquellos que sean igual a `a`.

Me invocan. `l` es una lista. `l` no es null. Voy a *evaluar* el elemento que me corresponde: `car l`. 

1) Si es un *atom*, la recursión será hacia el siguiente elemento de la lista. La recursión natural en este caso es `cdr l`. Si corresponde (`eq? (car l) a` es `#f`), recurro agregando `cons (car l)`.

2) Si es una *list*, debemos recurrir hacia *adentro*, ya que queremos remover `a` también de esta sublista. Para ello usamos `rember* a (car l)`. Agregamos esta lista con los `a` removidos, y recurrimos: `cons (rember* a (car l)) (rember* a (cdr l))`

1)

```
1.
((a b) a d)
car l: (a b)
cons (rember* a (car l)) (rember* a (cdr l)
cons (rember* a (a b)) (rember* a (a d))

2.
(a b)
car l: a
rember* a (cdr l)
rember* a (b)

3.
(b)
car l: b
cons (car l) (rember* a (cdr l))
cons b (rember* a ())

4.
()
null l
'()

3.
cons (rember* a ())
cons b ()

2.
rember* a (b)
(b)

1.
cons (rember* a (a b)) (rember* a (a d))
cons (b) (rember* a (a d))

5.
(a d)
car l: a
rember* a (cdr l)
rember* a (d)

6.
(d)
car l: d
cons (car l) (rember* a (cdr l))
cons d (rember* a ())

7.
()
null l
'()

6.
cons d (rember* a ())
cons d ()

5.
rember* a (d)
(d)

1.
cons (b) (rember* a (a d))
cons (b) (d)
((b) d)
```

Básicamente, además de la recursión natural, se recurre sobre el elemento actual si es necesario.

## `insertR* new old l`

Construiré una nueva lista. El elemento que analizo es `car l`. Pueden pasar tres cosas:

* `car l` es un `atom`, así que chequeo que sea igual a `old`. Si lo es, además de agregar `old` y recurrir con `insertR* new old (cdr l)`, agrego `new` después de `old`.
* `car l` es una lista, así que, antes de recurrir con `insertR* new old (cdr l)`, debo recurrir hacia *adentro* de `car l`. Agrego el resultad de esta recursión al de la recursión natural de `l`: `cons (insertR* new old (car l) (insertR* new old (cdr l))`.
* `l` es null. Como estaba construyendo una lista, deberé retornar `()`.

```
(define insertR* new old l
  (lambda (new old l)
    (cond
      ((null? l) '())
      ((atom? (car l))
        (cond
          ((eq? (car l) old)
            (cons old (cons new (insertR* new old (cdr l)))))
          (else
            (cons (car l) (insertR* new old (cdr l))))))
      (else
        (cons (insertR* new old (car l)) (insertR* new old (cdr l)))))))
```

## `occur* a l`

Voy a contar ocurrencias. Voy a construir un número. Voy a armar un número usando `add1` y voy a recurrir hacia adelante `cdr l`, y, cuando sea necesario, hacia abajo: `car l`.

```
(define occur*
  (lambda (a l)
    (cond
      ((null? l) 0)
      ((atom? (car l))
        (cond
          ((eq? (car l) a)
            (add1 (occur* a (cdr l))))
          (else
            (occur* a (cdr l)))))
      (else
        (+ (occur* a (car l)) (occur* a (cdr l)))))))
```

**Nota**: antes el *elemento típico* era sobre lo que se hacía `cons`. Ahora, también se hace `cons` sobre el resultado de la recursión *hacia abajo*.

## `member* a l`

Voy a verificar si algún atom de la lista cumple con una condición. 

Si el elemento actual (`car l`) es un atom:

* Si se cumple la condición, la recursión termina y el resultado es `#t`.
* Si no se cumple la condición, se recurre hacia adelante `cdr l`.

Si el elemento actual es una lista, se debe recurrir hacia abajo, permitiendo que la recursión termine en caso de que dicha recursión sea `#t`: `(or (member* a (car l)) (member* a (cdr l)))`.

```
(define member*
  (lambda (a l)
    (cond
      ((null? l) 0)
      ((atom? (car l))
        (cond
          ((eq? (car l) a) #t)
          (else (member* a (cdr l)))))
      (else
        (or (member* a (car l)) (member* a (cdr l)))))))
```

El libro plantea el siguiente código simplificado.

```
(define member*
  (lambda (a l)
    (cond
      ((null? l) 0)
      ((atom? (car l))
        (cond
          (or (eq? (car l) a)
            (member* a (cdr l)))))
      (else
        (or (member* a (car l)) (member* a (cdr l)))))))
```

## `eqlist? l1 l2`

Código del libro:

```
(define eqlist?
	(lambda (l1 l2)
		(cond
			((and
				(null? l1)
				(null? l2))
					#t)
			((and
				(null? l1)
				(atom? (car l2)))
					#f)
			((null? l1)
				#f)
			((and
				(atom? (car l1))
				(null? l2))
					#f)
			((and 
				(atom? (car l1))
				(atom? (car l2))
					(and
						(eqan? (car l1) (car l2))
						(eqlist? (cdr l1) (cdr l2))))
			((atom? (car l1))
				#f)
			((null? l2)
				#f)
			((atom? (car l2))
				#f)
			(else
				(and
					(eqlist? (car l1) (car l2))
					(eqlist? (cdr l1) (cdr l2)))))))
```