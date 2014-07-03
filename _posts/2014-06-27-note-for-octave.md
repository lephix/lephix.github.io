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
octave:2> PS1('>> ') % Change the prompt for the command line
>> 
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


### Plotting Data
~~~matlab
octave:15> x = [0:0.01:1]
octave:16> y1 = sin(4*pi*x)
octave:17> plot(x, y1) % we can see a graph with a sin(x) from 0 to 4pi
octave:18> y2 = cos(4*pi*x)
octave:19> plot(x, y2) % we can see a new graph with a cos(x) from 0 to 4pi
octave:20> hold on % this command will not clean the graph if a new graph is going to plot
octave:21> plot(x, y1, 'r') % there is a graph has the mixed sin(x) and cos(x) graph together will be shown. and the sin(x) will be drawn in red color.
octave:35> xlabel('time')
octave:36> ylabel('value')
octave:37> legend('sin', 'cos')
octave:38> title('my plot')
octave:40> print -dpng 'test.png' % output the graph to png format
octave:241 close % close the graph
octave:46> figure(1)
octave:47> plot(x, y1)
octave:48> figure(2)
octave:49> plot(x, y2)
octave:55> subplot(1,2,1) % divide plot to a 1x2 grid, and access the first element
octave:56> plot(x, y1)
octave:57> subplot(1,2,2) % access the second element
octave:58> plot(x, y2)
octave:60> axis([0.5, 1, -1, 1]) % change axis to x [0.5 to 1], y [-1 to 1]
octave:76> imagesc(rand(8:8))
octave:78> imagesc(rand(8:8)), colorbar, colormap gray % draw a colorbar and change the colormap to gray
octave:87> colormap default % change back to the normal colormap
octave:99> imagesc(magic(11)) % magic will return a magic matrix, and this can show the relationship between each element.
~~~

### Control statement: for , while, if
~~~matlab
octave:7> v = zeros(10,1)
v =

   0
   0
   0
   0
   0
   0
   0
   0
   0
   0

octave:8> for i=1:10
> v(i) = 2^i;
> end
octave:9> v
v =

      2
      4
      8
     16
     32
     64
    128
    256
    512
   1024
octave:12> i=1
i =  1
octave:13> while i<=5,
> v(i)=100,
> i=i+1,
> end;
octave:14> v
v =

    100
    100
    100
    100
    100
     64
    128
    256
    512
   1024
octave:16> if i==1,
> disp(1);
> elseif i==2,
> disp(2);
> else,
> disp(3)
> end;
 1
~~~

### Custom function

We can create a plain text file with name `test1.m`. Following is the content.

~~~matlab
function y = test1(x)
  y = x^2;
~~~

Than `cd` to the path of `test1.m`, and run the function.


~~~matlab
octave:1> test1(2)
y =  4
ans =  4
~~~

### Vectorization

Vectorization will make the computation more efficient.

For example, we have following vectors.
<div>
`u = [u1,u2,u3] , v = [v1,v2,v3] , w = [w1,w2,w3]`
</div>
Here is the implementation using iteration

~~~matlab
for j = 1:3,
  u(j) = 2 * v(j) + 5 w(j);
end
~~~

We can simplify it to do it as following

~~~matlab
u = 2 * v + 5 * w
~~~


<div>
Here is another example, If `Theta = [[Theta1],[Theta2],[Theta3]], x = [[x1],[x2],[x3]],
h_Theta(x) = sum_(j=0)^n Theta_j x_j`
</div>
After vertorization
<div>
`h_Theta(x) = Theta^T x`
</div>
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>