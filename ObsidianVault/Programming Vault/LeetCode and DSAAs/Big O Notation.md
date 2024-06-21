
![[Captura de Tela (97).png]]

Under the complexities are examples of algorithms of that complexity

Rules:
	1. assumed input >= 0. n is amount of input
	2. functions generally do more work for more input
	3. constants do not matter
	4. ignore lower order terms
	5. ignore the base of logs, since logs are scaled
	6. 2n = O(n) -> 2n pertence a O(n)
Semantical Rules:
	1. Constant time is achieved in operations where the amount of input does not matter
	2. logarithmic time usually happens when you divide or multiply ___I dont know what___. The base of the log is determined by how much you are multiplying/dividing the input.
	3. linear time is when the operation is done once per input
	4. quadratic time  = comparing everything to itself. Usually happens when one input is used with all other inputs + itself, that being true for all the other inputs as well.
	5. factorial time = happens when you have to iterate through all of the inputs over and over again, but the amount of inputs you need to iterate over is always less than the previous iteration. Until eventually the desired goal is achieved and the algorithm finishes its execution.

Big O Notaition:

O - this algo can only have equal or better efficiency than the previous one
o - this algo can only have better efficiency than the previous one
θ - this algo can only have same efficiency as the last one
Ω - this algo can only have equal or worse efficiency than the previous one
**ω** - this algo can only have worse efficiency than the last one
