---
layout: post
---
## Matrix

+ Operation

  + Addition and Subtraction

    Only matrices with same dimension could be added/substract. Doing the add/subtract means each number in the first matrix will add/subtract the number in the same position in the second matrix.

  + Scalar Multiplication

    A real number multiply a matrix means each of the number in the matrix will be multiplied with that real number.

  + Matrix Vector Multiplication

    Let say we are going to mulitply a 3x2 matrix *A* with an 2x1 vector *x*, the result is *y* which is a 3x1 vector. So to get *y[i]*, multiply *A*\'s *i*th row with elements of vector *x*, and add them up.

  + Matrix Matrix Multiplication

    Let say we are going to mulitply a m*n matrix *A* with an n*o matrix *B*, the result is *C* which is a m*o matrix. So the *i*th column of the *C* is obtained by multiplying *A* with the *i*th column of *B*. (for i=1,2,...,o)

    In short, Matrix and Matrix multiplication result is the combination of the result from Matrix with several Vectors.

  + Matrix Multiplication

    No commutative law for Matrix multiplication. *A* x *B* not equal to *B* x *A*

    Has association law for Matrix multiplication. (*A* x *B*) x *C* equal to *A* x (*B* x *C*)

  + Identity Matrix

    Always mark Identity matrix as *I*. *I* x *A* = *A* x *I* = *A*

    The size of the *I* is implict according to the context. Let say *A* is a 3x2 matrix, than *A* x *I*, the *I* has a 2x2 size. *I* x *A*, the *I* has a 3x3 size.

  + Matrix Inverse

    If *A* is an m x m matrix, and if it has an inverse, than A x A(-1) = A(-1) x A = I. //TODO change (-1) to superscript.

  + Matrix Transpose

    Let *A* be an m x n martrix, and let *B* = *A*(T). Then *B* is an n x m matrix, and *B*(ij) = *A*(ji). //TODO change (T) to superscript and (ij) to subscript.

### Vector

  Vector is a matrix with only on column.