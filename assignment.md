# Assignment 5: Basics of Interprocess Communication

## Race Condition
**What is a race condition?**  
It's when two processes try to acess the same resources at the same time.
The first one who gets on it loses, because the second one overwrites it.

## Disabling Interrupts
**Why is it impossible to achieve Mutual Exclusion via disabling interrupts on a multi-core machine?**
Because other processors can still run the code/acces the resources even if it's interrupted on one core.
All processors run the same at the same time.

**Why is it dangerous to give user processes the power to disable interrupts?**  
Because the user processes can run a endless loop without interrupts, so the computer is frozen.

## Peterson's Solution
### Scenario 1
- interested[0]=true , loser=0 , while gets skipped because interested[1]=false , 0 enters the critical region
- interested[1]=true , loser=1 , stuck in while because interested[0]=true
- interested[0]=false, process 1 can finally enter critical region

## Scenario 2
Both interested[0] and interested[1] are true, then the first process (0) sets loser on 0 but then the second
process(1) does it right after, this means loser=1 and the second process that enters (1) loses and is stuck
in the while loop.

## Strict alternation Scenario
Process 0 iterates 8 parts of the code, process 1 iterates 1 part of the code.
Proccess 0 goes in the critical region changes turn to 1 and leaves the region. Then he goes in the while loop
until 8 parts are done. Then process 1 enters the region and the scheduler switches to 0 then 0 goes 8 times in the
while loop. Then process 1 finally sets turn to 0. Then 0 leaves the while and enters the critical region and so on...

**What is the meaning of the variable loser in Peterson's solution? When does it have any effect?**
The loser value is the process id who has to wait in the while loop. It has an effect when process 0 is in the critical
region and process 1 wants to enter it.

## Peterson code with 3 processes
```
int loser // shared variable
Bool interested[3] // 3 processes only

void enter_region(int process) {
  int other1 = 2 - process - 1;
  int other2 = other1 + 1;
  interested[process] = True;
  loser = process;
  while (loser == process && interested[other1] && interested[other2]) ;
}
```
