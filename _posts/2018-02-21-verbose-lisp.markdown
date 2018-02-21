---
layout: post
title: "Verbose Lisp"
date: 2018-02-21 21:48:00 +0000
categories: [lisp, hello-world]
--- 

I'm preparing a lightning talk at work and have to come up with the most verbose yet simple chunk of Lisp code. So far I've come up with this:
```common-lisp
(let ((sentence "Hello, ~a!")
      (name "Runtime Revolution"))
  (funcall
       #'princ
       (format nil sentence name)))
```
It can be enclosed in a function and add a pair of `(  )`. Apart from that I can't think of any more extras to put in.
