---
layout: post
title: Note for learning Octave
---
This post is for recording memos of learning Octave.

### Installation

  Install cygwin. Then add octave, gnuplot, xterm, xorg-server, xinit.


### Operations
~~~matlab
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
~~~matlab
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
octave:4> whos % use whos or who to show all variables in memory
Variables in the current scope:

  Attr Name        Size                     Bytes  Class
  ==== ====        ====                     =====  ===== 
       ans         1x30                        30  char
       v           2x3                         48  double

Total is 36 elements using 78 bytes
octave:6> clear % use clear to clean all variables in memory
~~~

### Matrix related
~~~matlab
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
ans =p

   0.65891   0.64509   0.18112
   0.97003   0.22631   0.37190
octave:2> I = eye(2) % generate a 2x2 identity matrix
I =

Diagonal Matrix

   1   0
   0   1
octave:14> I(2,:) % ":" means every element along that row/column
ans =

   0   1
octave:15> A = [1 2; 3 4; 5 6]
A =

   1   2
   3   4
   5   6

octave:16> A(:,2) = [7;8;9] % change the second column's value
A =

   1   7
   3   8
   5   9

octave:17> A = [A, [10;11;12]] % append an column vector to right
A =

    1    7   10
    3    8   11
    5    9   12
octave:18> size(A) % return matrix A's size
ans =

   3   3
octave:19> A(:) % put all elements of A into a single vector
ans =

    1
    3
    5
    7
    8
    9
   10
   11
   12

octave:22> A
A =

   1   2
   3   4

octave:23> B
B =

   5   6
   7   8

octave:24> [A B] % append B to the right of A
ans =

   1   2   5   6
   3   4   7   8

octave:25> [A ; B] % append B to the bottom of A
ans =

   1   2
   3   4
   5   6
   7   8

~~~

### Display
~~~matlab
octave:1> e=pi % pi is a reserved variable
e =  3.1416
octave:2> disp(e)
 3.1416
octave:3> disp(sprintf('2 decimals: %0.2f', e))
2 decimals: 3.14
octave:12> hist(A) % display a histogram graphic for the matrix A
~~~

### Data Save/Load
~~~matlab
octave:1> v = rand(2,3)
v =

   0.45578   0.13747   0.11531
   0.43958   0.81255   0.42648

octave:2> save test.txt v % save v into test.txt in current directory
octave:8> load test.txt % load data from file test.txt into memory
octave:9> whos
Variables in the current scope:

  Attr Name        Size                     Bytes  Class
  ==== ====        ====                     =====  ===== 
       v           2x3                         48  double

Total is 6 elements using 48 bytes
octave:10> save test.txt v -ascii % save as ASCII type
~~~

### Computation Operations
~~~matlab
octave:1> A = [1 2 ; 3 4]
A =

   1   2
   3   4
octave:5> B = [10 11; 12 13]
B =

   10   11
   12   13
octave:6> A * B
ans =

   34   37
   78   85
octave:7> A .* B % Multiply each item in A to the corresponding item in B.
ans =

   10   22
   36   52
octave:8> A .^ 2 % Do the carry for each item
ans =

    1    4
    9   16
octave:9> -A % -1 * A
ans =

  -1  -2
  -3  -4
octave:10> A' % transpose of A
ans =

   1   3
   2   4
octave:15> [val, ind] = max(B(2,:)) % get the maximun number of second row, val is the maximun number and ind is the index of it
val =  13
ind =  2
octave:16> A < 3 % item wise operation, 1 means true and 0 means false
ans =

   1   1
   0   0
octave:20> [r, c] = find (A > 2) % r means row, c means column. These two array point to the position of items that satisfied with the condition.
r =

   2
   2

c =

   1
   2
octave:28> C = [1 2 3 4 ]
C =

   1   2   3   4

octave:29> sum(C)
ans =  10
octave:30> prod(C)
ans =  24
octave:31> sum(A,1) % sum all elements in each column. 1 means column wise.
ans =

   4   6
octave:32> sum(A,2) % sum all elements in each row. 2 means row wise.
ans =

   3
   7
octave:35> flipud(eye(3)) % Return a copy of eye(3) with the order of the rows reversed.
ans =

Permutation Matrix

   0   0   1
   0   1   0
   1   0   0
octave:38> pinv(A) % Return the pseudoinverse of A
ans =

  -2.00000   1.00000
   1.50000  -0.50000

octave:39> A * pinv(A)
ans =

   1.0000e+00  -4.4409e-16
   1.3323e-15   1.0000e+00

~~~