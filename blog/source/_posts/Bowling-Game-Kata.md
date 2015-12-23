title: Bowling Game Kata
date: 2015-12-23 23:10:02
categories:
- Programming
tags:
- Kata
- racket
---

#Bowing Game Kata

## Introduction

Kata definition from [Wiki](https://en.wikipedia.org/wiki/Kata)

> ![Solo training of kata](https://raw.githubusercontent.com/carwestsam/rollingBallKata/master/rolling_pic/440px-Iaido2.jpg)
>
> Kata (型 or 形 literally: "form"?), a Japanese word, are the detailed
> choreographed patterns of movements practised either solo or in pairs. The
> term form is used for the corresponding concept in non-Japanese martial arts
> in general.
>
> ...
>
> More recently, kata has come to be used in English in a more general or
> figurative sense, referring to any basic form, routine, or pattern of behavior
> that is practiced to various levels of mastery.

In these repo's context, Kata is kind of practise which should repeat overtimes.

As you can see, this is a programming Kata.

Writing the code for several time, with 100% concentrate, and Repeat overtimes,
improving your design over and over again. 

## Game Rules

![Bowing](https://raw.githubusercontent.com/carwestsam/rollingBallKata/master/rolling_pic/rolling_pic.002.jpg)


##Code

自己写的代码，经过了几次修改，最终的版本是用racket写的（因为racket有自带的自动化测试框架）

代码很短，直接粘贴源码好了

源代码：

```racket
#lang racket

(define (process-frame strikes)
  (cond
    ((= (car strikes) 10) (cons (apply + (take strikes 3)) (cdr strikes)))
    ((= (apply + (take strikes 2)) 10) (cons (apply + (take strikes 3)) (drop strikes 2)))
    ((cons (apply + (take strikes 2)) (drop strikes 2)))))

(define (process-last-frame strikes)
  (list (apply + strikes)))

(define (game strikes)
  (define (game-step frames score strikes)
    (if (= frames 10)
        (+ score (car (process-last-frame strikes)))
        (letrec ((result (process-frame strikes)))
          (game-step (+ frames 1) (+ score (car result)) (cdr result)))))
  (game-step 1 0 strikes))

(provide process-frame process-last-frame game)
```

测试代码：

```racket
#lang racket

(require rackunit "rolling.rkt")

(test-case "should process basic frame"
           (check-equal? (process-frame (list 1 2 3)) (list 3 3)))

(test-case "should process spare frame"
           (check-equal? (process-frame (list 2 8 3)) (list 13 3)))

(test-case "should process strike frame"
           (check-equal? (process-frame (list 10 2 3)) (list 15 2 3)))

(test-case "should process last frame"
           (check-equal? (process-last-frame (list 2 3 4)) (list 9)))

(test-case "shoudl process whole game"
           (define strikes (list 1 4 4 5 6 4 5 5 10 0 1 7 3 6 4 10 2 8 6))
           (check-equal? (game strikes) 133))

(test-case "shoudl process perfect game"
           (define strikes (list 10 10 10 10 10 10 10 10 10 10 10 10))
           (check-equal? (game strikes) 300))
```

## Related links

[programingpraxis](http://programmingpraxis.com/2009/08/11/uncle-bobs-bowling-game-kata/)
