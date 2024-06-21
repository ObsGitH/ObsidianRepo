
You need to be careful when debugging because the dubugger has very awkward behavior that I do not understand.

Here you will save problems that you faced when using the debugger and how you solved them.


1. You got stuck in the development of the blackjack game because the algorithm of the test: bet_system_test() was not able to access the input variable of the bet() method that belongs to the player struct, because while the debugger was executing the bet_system_set it was also executing the ante_bet_test() test. 
	- I believe that the problem occurred because the debugger executes test in parallel by default. So even tho I was controlling the execution of the bet_system_test the ante_bet_test was also being controlled in parallel which made rust panick due to the race condition of accessing the input variable simultaneously . That makes even more sense considering that in the ante_bet_test() I was also using the input variable to get input through the CLI, so I reached the input variable in bet_system_test while the ante_bet_test() was waiting for the input in CLI and therefore rust panick due to race condition error.
	- How did I solve it: I deleted the ante_bet_test() for the lib.rs module.