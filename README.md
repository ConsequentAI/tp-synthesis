# Theorem-proving based code synthesis
Linux binary demo of theorem proving-based code synthesis; for the simple case of MM

# Run instructions

Note: Change `python3.8` to the relevant one on your system.

```sh
virtualenv venv
source ./venv/bin/activate
pip install -r requirements.txt
export Z3_LIBRARY_PATH="venv/lib/python3.8/site-packages/z3/lib/"

# single cpu synthesis
./mm-dataset
```

Many CPU search for alternative configs:
```sh
./mm-dataset --procs 50 --timeout 20000
killall -9 mm-dataset
```

# Sample output

Uses the [z3 theorem prover](https://github.com/Z3Prover/z3) to search the space for alternative versions of Strassen's matrix multiplication. If you let it run it will search 2x2x2 --> 5x5x5.

```
(venv) $ ./mm-dataset 
Out to dir ./dataset with timeout 5000
Total:  282051
Limit (p, s):
        max non zeros (4, 4),
        min zeros (p = 4, s = 3)
        max zeros (p_a = 3, p_b = 3, s = 5)
Search started on 2024-05-09 09:30:33.421806
Solving (2x2x2 in 7 muls)..
Took 0:00:00.111973
SolveStatus.FOUND
{'p_0_1': 3, 'p_0_3': 1, 'p_0_6': 1, 'p_0_7': 1, 'p_1_2': 1, 'p_1_3': 1, 'p_1_4': 3, 'p_2_0': 1, 'p_2_2': 3, 'p_2_4': 1, 'p_2_5': 1, 'p_3_0': 1, 'p_3_1': 1, 'p_3_7': 1, 'p_4_0': 1, 'p_4_3': 1, 'p_4_4': 1, 'p_4_7': 1, 'p_5_3': 1, 'p_5_4': 1, 'p_5_6': 3, 'p_6_0': 1, 'p_6_5': 3, 'p_6_7': 1, 's_0_0': 3, 's_0_3': 3, 's_0_4': 1, 's_0_5': 3, 's_1_3': 1, 's_1_6': 3, 's_2_1': 3, 's_2_5': 3, 's_3_1': 1, 's_3_2': 3, 's_3_4': 1, 's_3_6': 3}


# Solves 2x2x2 in 7 muls and 18 adds (split as (10, 8))
# Time to discover = 0:00:00.111973
# Task was 2x2x2-7
# Search using symm:False-limit:True-p_limit:4-s_limit:4-fixed:[]


N = 2
M = 2
R = 2

def mm(A, B):
	S = [ [ 0 for _ in range(R) ] for _ in range(N) ]
	P0 = (A[1][1] + -A[0][1]) * (B[1][0] + B[1][1])
	P1 = (A[1][0] + A[1][1]) * -B[0][0]
	P2 = (A[0][0] + -A[1][0]) * (B[0][0] + B[0][1])
	P3 = (A[0][0] + A[0][1]) * B[1][1]
	P4 = (A[0][0] + A[1][1]) * (B[0][0] + B[1][1])
	P5 = A[1][1] * (B[0][0] + -B[1][0])
	P6 = A[0][0] * (B[1][1] + -B[0][1])

	S[0][0] = (P4 + -P0 + -P5 + -P3)
	S[0][1] = (-P6 + P3)
	S[1][0] = (-P1 + -P5)
	S[1][1] = (-P2 + -P6 + P4 + P1)
	return S
Saved: config-2x2x2.json
Saved: task-2x2x2.json
Saved: program to rslt-2x2x2.py
[SolveStatus.FOUND] Valid synthesis
Took 0.111973s, with 18 adds, rated at 1.0
Saved: ./dataset/task:2x2x2-7-config:6021639308605474583-time:5000ms.json
Limit (p, s):
        max non zeros (4, 5),
        min zeros (p = 4, s = 2)
        max zeros (p_a = 3, p_b = 3, s = 5)
Search started on 2024-05-09 09:30:33.604596
Solving (2x2x2 in 7 muls)..
Took 0:00:00.374549
SolveStatus.FOUND
{'p_0_2': 3, 'p_0_5': 3, 'p_0_7': 1, 'p_1_1': 1, 'p_1_4': 1, 'p_1_6': 3, 'p_2_0': 1, 'p_2_1': 1, 'p_2_4': 1, 'p_3_1': 3, 'p_3_2': 1, 'p_3_4': 1, 'p_3_7': 3, 'p_4_1': 1, 'p_4_3': 1, 'p_4_6': 3, 'p_4_7': 1, 'p_5_2': 1, 'p_5_3': 1, 'p_5_7': 1, 'p_6_0': 3, 'p_6_2': 3, 'p_6_4': 1, 'p_6_5': 3, 's_0_1': 3, 's_0_2': 1, 's_1_0': 3, 's_1_2': 1, 's_1_3': 1, 's_1_6': 1, 's_2_1': 1, 's_2_3': 1, 's_2_4': 3, 's_2_5': 1, 's_3_0': 1, 's_3_5': 1}


# Solves 2x2x2 in 7 muls and 18 adds (split as (10, 8))
# Time to discover = 0:00:00.374549
# Task was 2x2x2-7
# Search using symm:False-limit:True-p_limit:4-s_limit:5-fixed:[]


N = 2
M = 2
R = 2

def mm(A, B):
	S = [ [ 0 for _ in range(R) ] for _ in range(N) ]
	P0 = -A[1][0] * (-B[0][1] + B[1][1])
	P1 = A[0][1] * (B[0][0] + -B[1][0])
	P2 = (A[0][1] + A[0][0]) * B[0][0]
	P3 = (A[1][0] + -A[0][1]) * (B[0][0] + -B[1][1])
	P4 = (A[0][1] + A[1][1]) * (-B[1][0] + B[1][1])
	P5 = (A[1][0] + A[1][1]) * B[1][1]
	P6 = (-A[0][0] + -A[1][0]) * (B[0][0] + -B[0][1])

	S[0][0] = (-P1 + P2)
	S[0][1] = (P6 + P2 + -P0 + P3)
	S[1][0] = (P1 + P3 + P5 + -P4)
	S[1][1] = (P0 + P5)
	return S
Saved: config-2x2x2.json
Saved: task-2x2x2.json
Saved: program to rslt-2x2x2.py
[SolveStatus.FOUND] Valid synthesis
Took 0.374549s, with 18 adds, rated at 0.9999999999997384
Saved: ./dataset/task:2x2x2-7-config:2707872791552405856-time:5000ms.json
Limit (p, s):
        max non zeros (4, 6),
        min zeros (p = 4, s = 1)
        max zeros (p_a = 3, p_b = 3, s = 5)
Search started on 2024-05-09 09:30:34.049955
Solving (2x2x2 in 7 muls)..
Took 0:00:00.380788
SolveStatus.FOUND
{'p_0_0': 1, 'p_0_3': 1, 'p_0_4': 3, 'p_0_7': 1, 'p_1_2': 3, 'p_1_3': 1, 'p_1_4': 1, 'p_2_0': 1, 'p_2_2': 1, 'p_2_4': 1, 'p_2_5': 1, 'p_3_0': 3, 'p_3_5': 1, 'p_3_7': 1, 'p_4_3': 1, 'p_4_4': 3, 'p_4_6': 3, 'p_5_1': 3, 'p_5_3': 3, 'p_5_6': 3, 'p_5_7': 3, 'p_6_0': 1, 'p_6_1': 3, 'p_6_7': 1, 's_0_0': 3, 's_0_4': 1, 's_0_5': 1, 's_0_6': 1, 's_1_3': 3, 's_1_6': 3, 's_2_1': 3, 's_2_4': 3, 's_3_0': 1, 's_3_1': 1, 's_3_2': 1, 's_3_3': 1}


# Solves 2x2x2 in 7 muls and 18 adds (split as (10, 8))
# Time to discover = 0:00:00.380788
# Task was 2x2x2-7
# Search using symm:False-limit:True-p_limit:4-s_limit:6-fixed:[]


N = 2
M = 2
R = 2

def mm(A, B):
	S = [ [ 0 for _ in range(R) ] for _ in range(N) ]
	P0 = (A[0][0] + A[1][1]) * (-B[0][0] + B[1][1])
	P1 = (-A[1][0] + A[1][1]) * B[0][0]
	P2 = (A[0][0] + A[1][0]) * (B[0][0] + B[0][1])
	P3 = -A[0][0] * (B[0][1] + B[1][1])
	P4 = A[1][1] * (-B[0][0] + -B[1][0])
	P5 = (-A[1][1] + -A[0][1]) * (-B[1][1] + -B[1][0])
	P6 = (A[0][0] + -A[0][1]) * B[1][1]

	S[0][0] = (P4 + -P0 + P5 + P6)
	S[0][1] = (-P6 + -P3)
	S[1][0] = (-P1 + -P4)
	S[1][1] = (P2 + P0 + P3 + P1)
	return S
Saved: config-2x2x2.json
Saved: task-2x2x2.json
Saved: program to rslt-2x2x2.py
[SolveStatus.FOUND] Valid synthesis
Took 0.380788s, with 18 adds, rated at 0.9999999999995748
Saved: ./dataset/task:2x2x2-7-config:867069619140527992-time:5000ms.json
Limit (p, s):
        max non zeros (4, 7),
        min zeros (p = 4, s = 0)
        max zeros (p_a = 3, p_b = 3, s = 5)
Search started on 2024-05-09 09:30:34.501483
Solving (2x2x2 in 7 muls)..
Took 0:00:00.173362
SolveStatus.FOUND
{'p_0_1': 1, 'p_0_3': 1, 'p_0_6': 1, 'p_0_7': 1, 'p_1_0': 1, 'p_1_2': 1, 'p_1_4': 1, 'p_1_5': 1, 'p_2_1': 1, 'p_2_5': 1, 'p_2_7': 1, 'p_3_2': 3, 'p_3_4': 1, 'p_3_6': 1, 'p_4_2': 3, 'p_4_3': 1, 'p_4_6': 1, 'p_5_0': 3, 'p_5_1': 1, 'p_5_5': 3, 'p_6_1': 1, 'p_6_2': 1, 'p_6_5': 3, 'p_6_6': 1, 's_0_1': 1, 's_0_3': 1, 's_0_5': 3, 's_0_6': 1, 's_1_2': 1, 's_1_5': 1, 's_2_3': 3, 's_2_4': 1, 's_3_0': 1, 's_3_2': 3, 's_3_4': 3, 's_3_6': 3}


# Solves 2x2x2 in 7 muls and 18 adds (split as (10, 8))
# Time to discover = 0:00:00.173362
# Task was 2x2x2-7
# Search using symm:False-limit:True-p_limit:4-s_limit:7-fixed:[]


N = 2
M = 2
R = 2

def mm(A, B):
	S = [ [ 0 for _ in range(R) ] for _ in range(N) ]
	P0 = (A[1][1] + A[0][1]) * (B[1][0] + B[1][1])
	P1 = (A[1][0] + A[0][0]) * (B[0][0] + B[0][1])
	P2 = A[0][1] * (B[1][1] + B[0][1])
	P3 = -A[1][0] * (B[0][0] + B[1][0])
	P4 = (-A[1][0] + A[1][1]) * B[1][0]
	P5 = (-A[0][0] + A[0][1]) * -B[0][1]
	P6 = (A[0][1] + A[1][0]) * (B[1][0] + -B[0][1])

	S[0][0] = (P1 + -P5 + P6 + P3)
	S[0][1] = (P5 + P2)
	S[1][0] = (-P3 + P4)
	S[1][1] = (-P2 + P0 + -P6 + -P4)
	return S
Saved: config-2x2x2.json
Saved: task-2x2x2.json
Saved: program to rslt-2x2x2.py
[SolveStatus.FOUND] Valid synthesis
Took 0.173362s, with 18 adds, rated at 1.0
Saved: ./dataset/task:2x2x2-7-config:-3306416479156666722-time:5000ms.json
Limit (p, s):
        max non zeros (4, 8),
        min zeros (p = 4, s = -1)
        max zeros (p_a = 3, p_b = 3, s = 5)
Search started on 2024-05-09 09:30:34.744984
Solving (2x2x2 in 7 muls)..
Took 0:00:00.377488
SolveStatus.FOUND
{'p_0_2': 3, 'p_0_3': 1, 'p_0_6': 1, 'p_1_1': 1, 'p_1_5': 1, 'p_1_7': 1, 'p_2_0': 1, 'p_2_1': 3, 'p_2_5': 1, 'p_3_0': 3, 'p_3_2': 1, 'p_3_4': 3, 'p_3_5': 1, 'p_4_1': 3, 'p_4_3': 1, 'p_4_6': 1, 'p_4_7': 3, 'p_5_1': 1, 'p_5_2': 3, 'p_5_5': 1, 'p_5_6': 1, 'p_6_2': 1, 'p_6_4': 1, 'p_6_6': 1, 's_0_2': 1, 's_0_3': 1, 's_0_5': 1, 's_0_6': 1, 's_1_1': 1, 's_1_2': 1, 's_2_0': 1, 's_2_6': 1, 's_3_0': 1, 's_3_1': 1, 's_3_4': 3, 's_3_5': 3}


# Solves 2x2x2 in 7 muls and 18 adds (split as (10, 8))
# Time to discover = 0:00:00.377488
# Task was 2x2x2-7
# Search using symm:False-limit:True-p_limit:4-s_limit:8-fixed:[]


N = 2
M = 2
R = 2

def mm(A, B):
	S = [ [ 0 for _ in range(R) ] for _ in range(N) ]
	P0 = (A[1][1] + -A[1][0]) * B[1][0]
	P1 = A[0][1] * (B[0][1] + B[1][1])
	P2 = (-A[0][1] + A[0][0]) * B[0][1]
	P3 = (A[1][0] + -A[0][0]) * (-B[0][0] + B[0][1])
	P4 = (-A[0][1] + A[1][1]) * (B[1][0] + -B[1][1])
	P5 = (-A[1][0] + A[0][1]) * (B[1][0] + B[0][1])
	P6 = A[1][0] * (B[0][0] + B[1][0])

	S[0][0] = (P2 + P5 + P6 + P3)
	S[0][1] = (P2 + P1)
	S[1][0] = (P6 + P0)
	S[1][1] = (P0 + -P5 + -P4 + P1)
	return S
Saved: config-2x2x2.json
Saved: task-2x2x2.json
Saved: program to rslt-2x2x2.py
[SolveStatus.FOUND] Valid synthesis
Took 0.377488s, with 18 adds, rated at 0.9999999999996705
Saved: ./dataset/task:2x2x2-7-config:-1230382329177810507-time:5000ms.json
Limit (p, s):
        max non zeros (4, 9),
        min zeros (p = 4, s = -2)
        max zeros (p_a = 3, p_b = 3, s = 5)
Search started on 2024-05-09 09:30:35.192448
Solving (2x2x2 in 7 muls)..
^CTook 0:00:00.161498
SolveStatus.TIMEOUT
None
[SolveStatus.TIMEOUT] Invalid synthesis
Took Nones, with None adds, rated at None
Saved: ./dataset/task:2x2x2-7-config:5220440666340358567-time:5000ms.json
Limit (p, s):
        max non zeros (4, 10),
        min zeros (p = 4, s = -3)
        max zeros (p_a = 3, p_b = 3, s = 5)
Search started on 2024-05-09 09:30:35.421830
Solving (2x2x2 in 7 muls)..
Took 0:00:00.322754
SolveStatus.FOUND
{'p_0_0': 1, 'p_0_2': 3, 'p_0_4': 1, 'p_0_5': 1, 'p_1_1': 3, 'p_1_3': 1, 'p_1_6': 1, 'p_1_7': 1, 'p_2_1': 3, 'p_2_2': 1, 'p_2_5': 1, 'p_2_6': 3, 'p_3_0': 1, 'p_3_1': 3, 'p_3_5': 1, 'p_4_2': 1, 'p_4_3': 3, 'p_4_6': 1, 'p_5_1': 1, 'p_5_5': 1, 'p_5_7': 1, 'p_6_2': 1, 'p_6_4': 1, 'p_6_6': 1, 's_0_0': 1, 's_0_2': 1, 's_0_3': 3, 's_0_6': 1, 's_1_3': 1, 's_1_5': 1, 's_2_4': 3, 's_2_6': 1, 's_3_1': 1, 's_3_2': 1, 's_3_4': 1, 's_3_5': 1}


# Solves 2x2x2 in 7 muls and 18 adds (split as (10, 8))
# Time to discover = 0:00:00.322754
# Task was 2x2x2-7
# Search using symm:False-limit:True-p_limit:4-s_limit:10-fixed:[]


N = 2
M = 2
R = 2

def mm(A, B):
	S = [ [ 0 for _ in range(R) ] for _ in range(N) ]
	P0 = (A[0][0] + -A[1][0]) * (B[0][1] + B[0][0])
	P1 = (A[1][1] + -A[0][1]) * (B[1][1] + B[1][0])
	P2 = (-A[0][1] + A[1][0]) * (-B[1][0] + B[0][1])
	P3 = (A[0][0] + -A[0][1]) * B[0][1]
	P4 = (A[1][0] + -A[1][1]) * B[1][0]
	P5 = A[0][1] * (B[1][1] + B[0][1])
	P6 = A[1][0] * (B[0][0] + B[1][0])

	S[0][0] = (P0 + P2 + P6 + -P3)
	S[0][1] = (P5 + P3)
	S[1][0] = (-P4 + P6)
	S[1][1] = (P2 + P5 + P4 + P1)
	return S
Saved: config-2x2x2.json
Saved: task-2x2x2.json
Saved: program to rslt-2x2x2.py
[SolveStatus.FOUND] Valid synthesis
Took 0.322754s, with 18 adds, rated at 0.9999999999999978
```
