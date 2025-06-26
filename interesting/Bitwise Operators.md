https://www.youtube.com/watch?v=igIjGxF2J-w
-  Bit fields use bitwise operators.
-  Bitwise operators are used in, for instance in Unix file systems. They use bitwise operators for file permissions.

| permissions | owner (rwx) | group (rwx) | everyone (rwx) |
| ----------- | ----------- | ----------- | -------------- |
| 777         | 111         | 111         | 111            |
| 444         | 100         | 100         | 100            |
| 666         | 110         | 110         | 110            |

| 7   | 6   | 7 & 6 | 7 \| 6 |
| --- | --- | ----- | ------ |
| 1   | 1   | 1     | 1      |
| 1   | 1   | 1     | 1      |
| 1   | 0   | 0     | 1      |

=> 7 & 6 = 110 = 6

=> 7 | 6 = 111 = 7