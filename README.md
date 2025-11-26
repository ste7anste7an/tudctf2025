# Approximate Closest Vector

Recover a length-32 byte string (the flag) from four known polynomial evaluations $y_{50},y_{51},y_{52},y_{53}$ of the unique degree-31 interpolant through the 32 bytes.

This can be reduced to an approximate closest-vector problem (CVP) in a lattice so integer coefficients $c_0,\dots,c_{31}$ are simultaneously close to printable ASCII and reproduce the four known evaluations.

-  Build Lagrange basis polynomials $P_i(x)$ for $i=1..32$ so $P_i(j)=\delta_{ij}$.  
   Any interpolant satisfies $p(x)=\sum_i c_i P_i(x)$ and in particular $p(t)=\sum_i c_i P_i(t)$.

- Form the $32\times 4$ matrix $A$ with $A_{i,k}=P_i(50+k)$ for $k=0..3$.

-  Choose a weight $L$ and form the block matrix $M=[I_{32}|L A]$.  
   Convert$M$ to a lattice.

-  Build target vector $T=[\text{MID},\dots,\text{MID}|L\cdot y_{50},\dots,L\cdot y_{53}]$ where MID is the midpoint of printable ASCII (e.g. $(32+128)/2=80$).

-  Use  `latticeM.approximate_closest_vector(target)` to find a lattice vector $v$ close to $T$.  
   The first 32 entries of $v$ are candidate ASCII codes; the last 4 check the reconstructed evaluations (divide by $L$).

-  Convert the first 32 integers to characters to obtain the flag.

