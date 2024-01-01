# poly34
Solution of cubic and quartic equations C++

Cubic equation
--------------
Linear and quadratic equations with real coefficients are easy to solve. For the solution of the cubic equation we take a trigonometric Viete method, C++ code takes about two dozen lines. The roots of equation

x^3 + a*x^2 + b*x + c = 0

may be computed by the function

int   SolveP3(double *x,double a,double b,double c);

Here x is an array of size 3.

In the case of three real roots function returns the number 3, the roots themselves back in x[0],x[1],x[2].

Remark 1. Roots are not necessarily ordered!
If two roots are match, the function returns 2 and in the array x still there are a three numbers.

If the function returns 1, then x[0] is a real root and x[1] ± i*x[2] is a pair of coplex-conjugated roots.

Remark 2. Due to rounding errors pair of complex conjugate roots with a very small imaginary part can sometimes be a real root of multiplicity 2. For example, for equation x^3 - 5x^2 + 8x - 4 = 0 with roots 1,2,2 we obtain roots     1.0, 2.0Бi*9.6e-17. If the absolute value of the imaginary part of the root not greater than 1e-14, the SolveP3 itself replaces a pair on one valid double root, but the user must still be aware of the possibility of such a situation.

Quartic equation
----------------

For the solution of a quartic equation we take a Descartes-Euler method. Roots of the equation

x^4 + a*x^3 + b*x^2 + c*x + d = 0

may be computed by the function

int   SolveP4(double *x,double a,double b,double c,double d);

Here x is an array of size 4.

In the case of 4 real roots function returns the number 4, the roots themselves back in x[0],x[1],x[2],x[3].

In the case of 2 real and a pair of complex conjugate roots function returns the number 2, x[0],x[1] are the real roots and x[2] ± i*x[3] is a complex.

If the equation has two pairs of pairs of complex conjugate roots, the function returns 0, the root are x[0] ± i*x[1] and x[2] ± i*x[3].

Remark 3. Numerical experiments show that in some cases the resulting tolerance is quite large, up to 10-12. So at the end each found real root are specified with a single step of Newton's method.

Quintic equation
-----------------
All roots of the quintic equation

f(x) = x^5 + a*x^4 + b*x^3 + c*x^2 + d*x + e = 0

not greater than

brd = 1 + max( |a|, |b|, |c|, |d|, |e| ).

Quintic equation have at least one real root. To find it, starting with the interval [-brd, brd] make 6 "bisections". Then check the root using the Newton method.

Finding one real root x0, divide it original polynomial f(x) and find the roots of the resulting polynomial of degree 4.

The solution of a quintic equation may be computed by the function

int   SolveP5(double *x,double a,double b,double c,double d, double e);

Here x is an array of size 5.

In the case of 5 real roots function returns the number 5, the roots themselves back in x[0],x[1],x[2],x[3], x[4].

In the case of 3 real and a pair of complex conjugate roots function returns the number 3, x[0],x[1],x[2] are the real roots and x[3] ± i*x[4] is a complex.

If the equation has one real root and two pairs of pairs of complex conjugate roots, the function returns 1, x[0] is a real root and x[1] ± i*x[2] , x[3] ± i*x[4] are a complex roots.

C++-realization

The package consists of two files: poly34.h, poly34.cpp. It does not require any additional libraries. From standard include-files only connected math.h. Dynamic memory allocation is not used.
Solution of cubic equations is performed in a single function SolveP3. To solve the equations of degree 4 are three auxiliary functions:

void  CSqrt( double x, double y, double &a, double &b);  // returns as a+i*s,  sqrt(x+i*y)

int   SolveP4Bi(double *x, double b, double d);	         // solve equation x^4 + b*x^2 + d = 0

int   SolveP4De(double *x, double b, double c, double d);// solve equation x^4 + b*x^2 + c*x + d = 0

The first is to extract the square root of a complex number: a+i*s = sqrt(x+i*y).

Second to solve the biquadratic equation, the third for the solutions of the depressed equation.
Remark 4. As in the case of cubic equations the root of multiplicity 2 or a pair of very close real roots can be shown as a pair of complex conjugate roots with small imaginary part.

Solution of quintic equations is performed in a function SolveP5:

int   SolveP5(double *x,double a,double b,double c,double d,double e);// solve equation x^5 + a*x^4 + b*x^3 + c*x^2 + d*x + e = 0
