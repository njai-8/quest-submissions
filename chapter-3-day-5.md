#### For today's quest, you will be looking at a contract and a script. You will be looking at 4 variables (a, b, c, d) and 3 functions (publicFunc, contractFunc, privateFunc) defined in SomeContract. In each AREA (1, 2, 3, and 4), I want you to do the following: for each variable (a, b, c, and d), tell me in which areas they can be read (read scope) and which areas they can be modified (write scope). For each function (publicFunc, contractFunc, and privateFunc), simply tell me where they can be called.

|   | Area 1 | Area 2 | Area 3 | Area 4 |
|---|:------:|:------:|:------:|:------:|
| a |R/W     |R/W     |R/W     |R       |
| b |R/W     |R       |R       |R       |
| c |R/W     |R       |R       |X       |
| d |R/W     |X       |X       |X       |


|                | Area 1 | Area 2 | Area 3 | Area 4 |
|----------------|:------:|:------:|:------:|:------:|
| publicFunc()   |YES     |YES     |YES     |YES     |
| contractFunc() |YES     |YES     |YES     |NO      |
| privateFunc()  |YES     |NO      |NO      |NO      |
