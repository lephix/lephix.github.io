---
layout: post
title: Note for learning Octave
---
### Installation

  Install cygwin. Then add octave, gnuplot, xterm, xorg-server, xinit


### Operations

~~~
octave:1> 5+6
ans =  11
octave:2> 1/2
ans =  0.50000
octave:3> 2^6
ans =  64
octave:4> 6*7
ans =  42
octave:5> 1 == 2
ans = 0
octave:6> 1 ~= 2
ans =  1
octave:7> 1 && 0
ans = 0
octave:8> 1 || 0
ans =  1
octave:9> xor(1, 0)
ans =  1
~~~

### Variables
~~~
octave:1> a = 1
a =  1
octave:2> b = 'str'
b = str
octave:3> c = 1.2; % semicolon supressing output
octave:4> d = 'OK' % comment
d = OK
octave:5> e=pi % pi is a reserved variable
e =  3.1416
octave:6> e
e =  3.1416
octave:9> format long
octave:10> e
e =  3.14159265358979
octave:11> format short
octave:12> e
e =  3.1416
~~~

### Matrix related
~~~
octave:13> A = [1,2; 3,4; 5,6;]  % 3x2 Matrix
A =

   1   2
   3   4
   5   6
octave:15> A = [1,2;  % Could do it in separate lines
> 3,4]
A =

   1   2
   3   4
octave:16> V = [1;2;3;4]  % 4x1 colomn vector
V =

   1
   2
   3
   4
octave:21> v = 1:5
v =

   1   2   3   4   5

octave:22> v = 1:0.2:3  % 1x11 row vector, start from 1, step with 0.2, end at 3
v =

 Columns 1 through 8:

    1.0000    1.2000    1.4000    1.6000    1.8000    2.0000    2.2000    2.4000

 Columns 9 through 11:

    2.6000    2.8000    3.0000
octave:22> ones(2,3)  % generate a 2x3 matrix of all 1
ans =

   1   1   1
   1   1   1
octave:23> c = 8*ones(2,3)
c =

   8   8   8
   8   8   8
octave:24> zeros(1,3)  % generate a 1x3 matrix of all 0
ans =

   0   0   0
octave:25> rand(2,3)  % generate a 2x3 martix filled with random number
ans =

   0.65891   0.64509   0.18112
   0.97003   0.22631   0.37190
octave:2> eye(2) % generate a 2x2 identity matrix
ans =

Diagonal Matrix

   1   0
   0   1

~~~

### Display
~~~
octave:1> e=pi % pi is a reserved variable
e =  3.1416
octave:2> disp(e)
 3.1416
octave:3> disp(sprintf('2 decimals: %0.2f', e))
2 decimals: 3.14
~~~