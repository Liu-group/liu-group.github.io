---
layout: post
comments: true
title: Tutorial 13. Math courses/books useful for theo/comp chem students.
---

### Learning objectives
* Learn about a few math books/courses that may be useful for theo/comp chem graduate students.

<span style="color:red">**Disclaimer**</span> Different subfields of theoretical/computational chemistry may need very different math tools. I will humbly recommend a few math books/courses I find useful for  **quantum chemistry** method development.

### Which math courses should I take? / Which math books should I read?
I had these questions as a graduate student. As a new PI, I have got these questions multiple times from students. Hence, I think this must be a common question among students new to theoretical/computational chemistry.

Instead of directly recommending books/courses, I will list questions I had as a student and then point to courses/books where I found answers.

#### Mysterious iterative methods
Before reading the source codes of real quantum chemistry packages, I used to think that:
* Hartree-Fock SCF is solved by simple iterations, as shown in the textbook.
* Linear equations are solved with LU decomposition.

After reading the source codes, I got questions like:
* *Why is SCF never solved with simple iterations? But with DIIS?*
* *What are DIIS, Davidson, CG algorithms?*
* *What do the LAPACK function names stand for? What are the underlying algorithms?*
* *What is a preconditioner?*

Obviously, the linear algebra methods used in modern quantum chemistry codes are different from what we learned from undergraduate-level linear algebra courses.

Courses/books related to understanding numerical linear algebra:
* **Courses**: Numerical linear algebra / Numerical analysis or equivalent. The course name varies in different universities.
* **Books**:
	* Van Loan, Charles F., and G. Golub. "Matrix computations (Johns Hopkins studies in mathematical sciences)." (1996).

	<img src="https://jhupbooks.press.jhu.edu/public/covers/9781421408590.jpg" alt="Matrix Computation" width="150"/>
	* Trefethen, Lloyd N., and David Bau III. Numerical linear algebra. Vol. 50. Siam, 1997.

		<img src="https://pictures.abebooks.com/isbn/9780898713619-us.jpg" alt="Matrix Computation" width="150"/>

#### Optimization jargon
Lots of jargon are printed in the geometry optimization output files, such as
* Trust region, trust radius
* BFGS, L-BFGS
* Quasi-Newton

How do these influence the geometry optimization outcome? How can we adjust these parameters?

Courses/books related to understanding optimization concepts:
* **Courses**: Numerical Optimization / Convex Optimization. Course name varies in different universities
* **Books**:
	* Gill, Philip E., Walter Murray, and Margaret H. Wright. Practical optimization. Society for Industrial and Applied Mathematics, 2019

	<img src="https://images-na.ssl-images-amazon.com/images/I/41xpaVF6c5L._SX415_BO1,204,203,200_.jpg" alt="Practical optimization" width="150"/>
	* Boyd, Stephen, Stephen P. Boyd, and Lieven Vandenberghe. Convex optimization. Cambridge university press, 2004.

	<img src="https://web.stanford.edu/~boyd/cvxbook/bv_cvxbook_cover.jpg" alt="Convex optimization" width="150"/>

**Side comments:** I also found these books/courses help me understand techniques used in machine learning: for supervised learning, we are always doing optimization.

### Fear of missing out (FOMO) on a math course
Not sure whether this is a common feeling among theoretical chemistry students, but I used to have this type of FOMO as a student. I suspect that some math courses I hadn't taken would be important for my research at some point. Therefore, I took more and more but later found them not closely related to my research. These courses are interesting, but we should also be wary about the time cost.

Rather than taking a math course whose usefulness to you is unclear, you may want to think differently. Be confident about your ability to learn any new math topic when you need it. We are all lifelong learners. WAs long as we continue this career path, we will always encounter new math or technology that we need to use but have never used. It is never too late to learn it when you find you need it. Therefore, don't worry about missing out on one math course at the moment.

### Take home message
* FOMO on a math course is not necessary
* Different subfields need very different math tools.
* Conserve your time and energy. Consult your advisor and read the books/take the courses most closely related to your research, i.e., answering questions you meet in your research.
