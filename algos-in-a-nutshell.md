#Chapter 1
	- balance your binary trees, idiot

#Chapter 2
	- instance (of a problem): a particular input data set to which a program is applied
	- how you encode the instance effects performance
	- describe behavior of an algo: rate of growth of execution time as function of size of input problem instance
	- a program is run on a platform:
		- the computer: cpu, data cache, fpu
		- language: compiler, interpreter, lanaguage settings
		- operating system
		- background processes

	- three performance cases: worst, average, best [runtime behavior]
	- worst case is often the easiest analysis
	- Average case work done by an algorithm - formula on p. 22
		- T_average_case(n) = 1/abs(S_n) * (sum(t(s_i) * Pr{s_i})) for all s_i in S_n)
			- n = size of instance
			- S_n = set of all instances
			- t() = work done by algorithm on the instance
			- Pr{} = probability distribution function that s_i will present itself as a instance

	- performance families
		- constant
		- log
		- sublinear
		- linear
		- nlog(n)
		- quadratic
		- exponential
	- Contant time
		- O(1)
		- comparing two numbers of the a size k e.g. two k-bit numbers
	- Log
		- O(log(n))
		- binary search
	- Sublinear
		- O(n^d) where d < 1
	- Linear
		-	O(n)
		- addition
	- nlog(n)
		- O(nlog(n))
		- recursive algos
	- quadratic
		- O(n^2)
		- multiplication
	
#Chapter 3
	- design pattern: a proven solution to a commonly occuring problem
	- Algorithm pattern format
		- name
		- synopsis
		- forces: description of the properties of the problem/solution
		- solution
		- consequences: advantages, disadvantages, anti-patterns
		- analysis: performance data, behaviors
		- related algorithms
	- floating point implementation
	- memory allocation

#Chapter 4
	- 

	
