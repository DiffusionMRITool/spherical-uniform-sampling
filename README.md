This code is used for dMRI scheme sampling.

### Setup

1. Clone this repository 
2. Install dependencies
```
pip install -r requirements.txt
```
Note that you will need to acquire a license to use GUROBI for solving discrete problems here. For more information, see:
+ https://pypi.org/project/gurobipy/
+ https://www.gurobi.com/academia/academic-program-and-licenses/
+ https://www.gurobi.com/free-trial/

### Quick-start tutorial 

You can check CLI program with option `-h` for help message.

For a example single shell sampling pipeline, we will first generate a scheme with 30 points and then apply flipping and ordering to it.

1. Generate a scheme
```bash
python ./src/qspace_direction/direction_generation.py --output scheme.txt -n 30
```

2. Flip the resulting scheme
```bash
python ./src/qspace_direction/direction_flip.py --input scheme.txt --output flipped.txt
```

3. Order the resulting scheme
```bash
python ./src/qspace_direction/direction_order.py flipped.txt --output flipped_ordered.txt
```

You can check `flipped_ordered.txt` for the final result. 

For a example multiple shell sampling pipeline, we will first generate a scheme with $90\times 3$ points and then apply flipping and ordering to it.

1. Generate a scheme
```bash
python ./src/qspace_direction/direction_generation.py --output scheme.txt -n 90,90,90
```

2. Flip the resulting scheme
```bash
python ./src/qspace_direction/direction_flip.py --input scheme_shell0.txt,scheme_shell1.txt,scheme_shell2.txt --output flipped.txt 
```

3. Order the resulting scheme
We need to concatenate 3 shells to make a bvec file.
```bash
cat flipped_shell0.txt flipped_shell0.txt flipped_shell0.txt > bvec.txt
```
Then a bval file is needed, here we create one with bvals 1000, 2000, 3000 for each shell.
```bash
perl -e '$count=10; while ($count>0) { print "1000\n"; $count--; }
         $count=10; while ($count>0) { print "2000\n"; $count--; }
         $count=10; while ($count>0) { print "3000\n"; $count--; }
' > bval.txt
```
Finally we run our ordering script.
```bash
python ./src/qspace_direction/direction_order.py bvec.txt bval.txt --output flipped_ordered.txt
```

### References