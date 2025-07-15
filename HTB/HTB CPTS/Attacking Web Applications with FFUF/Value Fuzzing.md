Once we fuzz for parameters anf ind some, we can fuzz for values to complete the puzzle

For instance, if the parameter is id, then we can fuzz numbers from 1 to 1000

for i in $(seq 1 1000); do echo $i >> ids.txt; done
