
<a id='Permutations'></a>

<a id='Permutations-1'></a>

# Permutations

- [Permutations](index.md#Permutations)

<a id='Perms' href='#Perms'>#</a>
**`Perms`** &mdash; *Module*.



This package implements permutations and some functions of them. It depends only  on  the  package  combinat. 

This  package  follows  the  design  of  permutations  in tha GAP language. `Perm`s  are permutations  of the  set `1:n`,  represented internally  as a vector  of `n`  integers holding  the images  of `1:n`.  The integer `n` is called  the degree  of the  permutation. In  this package,  as in  GAP (and contrary  to the philosophy of Magma or the package `Permutations.jl`), two permutationsof  different  degrees  can  be  multiplied (the resulr has the lerger  degree). Two permutations  are equal if  and only if  they move the same points in the same way, so two permutations of different degree can be equal; the degree is thus an implementation detail so usually it should not be used. One should rather use the function `largest_moved_point`.

This  design makes it  easy to multiply  permutations coming from different groups, like a group and one of its subgroups. It has a negligible overhead compared to the design where the degree is fixed.

The default constructor for a permutation uses the list of images of `1:n`, like  `Perm([2,3,1,5,4])`.  Often  it  is  more  convenient  to  use  cycle decompositions:    the   above   permutation    has   cycle   decomposition `(1,2,3)(4,5)`    thus   can   be    written   `Perm(1,2,3)*Perm(4,5)`   or `perm"(1,2,3)(4,5)"`  (this last form  can parse a  permutation coming from GAP  or the default printing at the REPL).  The list of images of `1:n` can be  recovered from the  permutation by the  function `vec`; note that equal permutations with different degrees will have different `vec`.

The  complete type of a permutation  is `Perm{T}` where `T<:Integer`, where `Vector{T}`  is the type of the vector which holds the image of `1:n`. This can  be used to save space or  time. For instance `Perm{UInt8}` can be used for  Weyl groups of rank≤8 since they permute  at most 240 roots. If `T` is not  specified we  take it  to be  `Int16` since  this is a good compromise between   speed,  compactness  and  possible  size  of  `n`.  One  can  mix permutations of different integer types; they are promoted to the wider one when multiplying.

**Examples of operations with permutations**

```julia-repl
julia> a=Perm(1,2,3)
(1,2,3)

julia> vec(a)
3-element Vector{Int16}:
 2
 3
 1

julia> a==Perm(vec(a))
true

julia> b=Perm(1,2,3,4)
(1,2,3,4)

julia> a*b     # product
(1,3,2,4)

julia> inv(a)  # inverse
(1,3,2)

julia> a/b     # quotient  a*inv(b)
(3,4)

julia> a\b     # left quotient inv(a)*b
(1,4)

julia> a^b     # conjugation inv(b)*a*b
(2,3,4)

julia> b^2     # square
(1,3)(2,4)

julia> 1^a     # image by a of point 1
2

julia> one(a)  # trivial permutation
()

julia> sign(a) # signature of permutation
1

julia> order(a) # order (least trivial power) of permutation
3

julia> largest_moved_point(a)
3

julia> smallest_moved_point(a)
1

julia> Perm{Int8}(a) # convert a to Perm{Int8}
Perm{Int8}: (1,2,3)

julia> Matrix(b)  # permutation matrix of b
4×4 Matrix{Bool}:
 0  1  0  0
 0  0  1  0
 0  0  0  1
 1  0  0  0
```

```julia-rep1
julia> randPerm(10) # random permutation of 1:10
(1,8,4,2,9,7,5,10,3,6)
```

`Perm`s have methods `copy`, `hash`, `==`, so they can be keys in hashes or elements  of sets; two permutations are equal  if they move the same points to  the same images. They have methods `cmp`, `isless` (lexicographic order on   moved  points)  so  they  can  be  sorted.  `Perm`s  are  scalars  for broadcasting.

Other   methods   on   permutations   are  `cycles,  cycletype,  reflength, mappingPerm, sortPerm, Perm_rowcol`.

No  method is given in  this package to enumerate  `Perm`s; you can use the method   `arrangements`  from   `Combinat`  or   iterate  the  elements  of `symmetric_group` with `PermGroups`.


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L1-L118' class='documenter-source'>source</a><br>

<a id='Perms.Perm' href='#Perms.Perm'>#</a>
**`Perms.Perm`** &mdash; *Type*.



`struct Perm{T<:Integer}`

A  Perm represents a permutation  of the set `1:n`  and is implemented by a `struct` with one field, a `Vector{T}` holding the images of `1:n`.

```julia-repl
julia> p=Perm(Int16[1,3,2,4])
(2,3)

julia> vec(p)
4-element Vector{Int16}:
 1
 3
 2
 4
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L127-L144' class='documenter-source'>source</a><br>

<a id='Perms.Perm-Tuple{Vararg{Integer}}' href='#Perms.Perm-Tuple{Vararg{Integer}}'>#</a>
**`Perms.Perm`** &mdash; *Method*.



`Perm{T}(x::Integer...)where T<:Integer`

returns  a cycle.  For example  `Perm{Int8}(1,2,3)` constructs the cycle    `(1,2,3)` as a `Perm{Int8}`. If omitted `{T}` is taken to be `{Int16}`.


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L155-L160' class='documenter-source'>source</a><br>

<a id='Perms.Perm-Tuple{AbstractMatrix{<:Integer}}' href='#Perms.Perm-Tuple{AbstractMatrix{<:Integer}}'>#</a>
**`Perms.Perm`** &mdash; *Method*.



`Perm{T}(m::AbstractMatrix)` If  `m` is a  permutation matrix, returns  the corresponding permutation of type `T`. If omitted, `T` is taken to be `Int16`.

```julia-repl
julia> m=[0 1 0;0 0 1;1 0 0]
3×3 Matrix{Int64}:
 0  1  0
 0  0  1
 1  0  0

julia> Perm(m)
(1,2,3)
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L230-L245' class='documenter-source'>source</a><br>

<a id='Perms.Perm-Tuple{AbstractVector, AbstractVector}' href='#Perms.Perm-Tuple{AbstractVector, AbstractVector}'>#</a>
**`Perms.Perm`** &mdash; *Method*.



`Perm{T}(l::AbstractVector,l1::AbstractVector)`

returns `p`, a `Perm{T}`, such that `l1^p==l` if such a `p` exists; returns `nothing` otherwise. If not given `{T}` is taken to be `{Int16}`. Needs the elements of `l` and `l1` to be sortable.

```julia-repl
julia> Perm([0,2,4],[4,0,2])
(1,3,2)
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L258-L269' class='documenter-source'>source</a><br>

<a id='Perms.Perm-Tuple{AbstractMatrix, AbstractMatrix}' href='#Perms.Perm-Tuple{AbstractMatrix, AbstractMatrix}'>#</a>
**`Perms.Perm`** &mdash; *Method*.



`Perm{T}(m::AbstractMatrix,m1::AbstractMatrix;dims=1)`

returns  `p`, a `Perm{T}`, which  permutes the rows of  `m1` (the coluns of `m1`  if `dims=2`)  to bring  them to  those of  `m`, if such a `p` exists; returns  `nothing` otherwise. If not given  `{T}` is taken to be `{Int16}`. Needs the elements of `m` and `m1` to be sortable.

```julia-repl
julia> Perm([0 1 0;0 0 1;1 0 0],[1 0 0;0 1 0;0 0 1];dims=1)
(1,3,2)

julia> Perm([0 1 0;0 0 1;1 0 0],[1 0 0;0 1 0;0 0 1];dims=2)
(1,2,3)
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L278-L293' class='documenter-source'>source</a><br>

<a id='Perms.@perm_str' href='#Perms.@perm_str'>#</a>
**`Perms.@perm_str`** &mdash; *Macro*.



@perm"..."

make a `Perm` from a string; allows GAP-style `perm"(1,2)(5,6,7)(4,9)"`


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L183-L187' class='documenter-source'>source</a><br>

<a id='Perms.largest_moved_point-Tuple{Perm}' href='#Perms.largest_moved_point-Tuple{Perm}'>#</a>
**`Perms.largest_moved_point`** &mdash; *Method*.



`largest_moved_point(a::Perm)` is the largest integer moved by a


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L350' class='documenter-source'>source</a><br>

<a id='Perms.smallest_moved_point' href='#Perms.smallest_moved_point'>#</a>
**`Perms.smallest_moved_point`** &mdash; *Function*.



`smallest_moved_point(a::Perm)` is the smallest integer moved by a


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L353' class='documenter-source'>source</a><br>

<a id='Base.:^-Tuple{AbstractVector, Perm}' href='#Base.:^-Tuple{AbstractVector, Perm}'>#</a>
**`Base.:^`** &mdash; *Method*.



`Base.:^(l::AbstractVector,p::Perm)` 

returns `l` permuted by `p`, a vector `r` such that `r[i^p]==l[i]`

**Examples**

```julia-repl
julia> [5,4,6,1,7,5]^Perm(1,3,5,6,4)
6-element Vector{Int64}:
 1
 4
 5
 5
 6
 7
```

note that we follow here the convention for the GAP function `Permuted`, but this has the consequence that `sort(a)==a^inv(Perm(sortperm(a)))`.


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L418-L437' class='documenter-source'>source</a><br>

<a id='Perms.sortPerm' href='#Perms.sortPerm'>#</a>
**`Perms.sortPerm`** &mdash; *Function*.



for convenience: `sortPerm(a)=Perm(sortperm(a))`


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L360' class='documenter-source'>source</a><br>

<a id='Perms.randPerm' href='#Perms.randPerm'>#</a>
**`Perms.randPerm`** &mdash; *Function*.



`randPerm([T,]n::Integer)` a random permutation of `1:n` of type `T`. If omitted `T` is taken to be `Int16`


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L364-L367' class='documenter-source'>source</a><br>

<a id='Perms.orbit-Tuple{Perm, Integer}' href='#Perms.orbit-Tuple{Perm, Integer}'>#</a>
**`Perms.orbit`** &mdash; *Method*.



orbit(a::Perm,i::Integer) returns the orbit of a on i


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L510-L512' class='documenter-source'>source</a><br>

<a id='Perms.orbits-Tuple{Perm}' href='#Perms.orbits-Tuple{Perm}'>#</a>
**`Perms.orbits`** &mdash; *Method*.



`orbits(a::Perm,d::Vector=1:length(a.d))` 

returns the orbits of `a` on domain `d`

**Example**

```julia-repl
julia> orbits(Perm(1,2)*Perm(4,5),1:5)
3-element Vector{Vector{Int16}}:
 [1, 2]
 [3]
 [4, 5]
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L524-L537' class='documenter-source'>source</a><br>

<a id='Perms.order' href='#Perms.order'>#</a>
**`Perms.order`** &mdash; *Function*.



`order(a::Perm)` is the order of the permutation a


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L630-L632' class='documenter-source'>source</a><br>

<a id='Perms.cycles-Tuple{Perm}' href='#Perms.cycles-Tuple{Perm}'>#</a>
**`Perms.cycles`** &mdash; *Method*.



`cycles(a::Perm)` returns the non-trivial cycles of `a`

**Example**

```julia-repl
julia> cycles(Perm(1,2)*Perm(4,5))
2-element Vector{Vector{Int16}}:
 [1, 2]
 [4, 5]
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L553-L562' class='documenter-source'>source</a><br>

<a id='Perms.cycletype-Tuple{Perm}' href='#Perms.cycletype-Tuple{Perm}'>#</a>
**`Perms.cycletype`** &mdash; *Method*.



`cycletype(a::Perm;domain=1:length(a.d),trivial=false)`

returns  the  partition  of  `maximum(domain)`  associated to the conjugacy class of `a` in the symmetric group of `domain`, with ones removed (thus it does  not  depend  on  `domain`  but  just  on  the  moved  points)  unless `trivial=true`.

**Example**

```julia-repl
julia> cycletype(Perm(1,2)*Perm(4,5))
2-element Vector{Int64}:
 2
 2

julia> cycletype(Perm(1,2)*Perm(4,5);trivial=true)
3-element Vector{Int64}:
 2
 2
 1

julia> cycletype(Perm(1,2)*Perm(4,5);trivial=true,domain=1:6)
4-element Vector{Int64}:
 2
 2
 1
 1
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L581-L609' class='documenter-source'>source</a><br>

<a id='Perms.support' href='#Perms.support'>#</a>
**`Perms.support`** &mdash; *Function*.



`support(a::Perm)` is the set of all points moved by `a`


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L356' class='documenter-source'>source</a><br>

<a id='Base.sign' href='#Base.sign'>#</a>
**`Base.sign`** &mdash; *Function*.



`sign(a::Perm)` is the signature of  the permutation `a`


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L657' class='documenter-source'>source</a><br>

<a id='Base.Matrix-Tuple{Perm, Any}' href='#Base.Matrix-Tuple{Perm, Any}'>#</a>
**`Base.Matrix`** &mdash; *Method*.



`Matrix(a::Perm,n=length(a.d))`  the  permutation matrix  for `a`  operating on  `n` points (by default, the degree of `a`). If given, `n` should be larger than `largest_moved_point(a)`.

```julia-repl
julia> Matrix(Perm(2,3,4),5)
5×5 Matrix{Bool}:
 1  0  0  0  0
 0  0  1  0  0
 0  0  0  1  0
 0  1  0  0  0
 0  0  0  0  1
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L213-L227' class='documenter-source'>source</a><br>

<a id='Base.:^-Tuple{AbstractMatrix, Perm}' href='#Base.:^-Tuple{AbstractMatrix, Perm}'>#</a>
**`Base.:^`** &mdash; *Method*.



`Base.:^(m::AbstractMatrix,p::Perm;dims=1)`

Applies the permutation `p` on the lines, columns or both of the matrix `m` depending on the value of `dims`

```julia-repl
julia> m=[3*i+j for i in 0:2,j in 1:3]
3×3 Matrix{Int64}:
 1  2  3
 4  5  6
 7  8  9

julia> p=Perm(1,2,3)
(1,2,3)

julia> m^p
3×3 Matrix{Int64}:
 7  8  9
 1  2  3
 4  5  6

julia> ^(m,p;dims=2)
3×3 Matrix{Int64}:
 3  1  2
 6  4  5
 9  7  8

julia> ^(m,p;dims=(1,2))
3×3 Matrix{Int64}:
 9  7  8
 3  1  2
 6  4  5
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L444-L478' class='documenter-source'>source</a><br>

<a id='Base.:^-Tuple{AbstractMatrix, Tuple{Perm, Perm}}' href='#Base.:^-Tuple{AbstractMatrix, Tuple{Perm, Perm}}'>#</a>
**`Base.:^`** &mdash; *Method*.



`Base.:^(m::AbstractMatrix,p::Tuple{Perm,Perm})`

given  a tuple `(p1,p2)` of  `Perm`s, applies `p1` to  the lines of `m` and `p2` to the columns of `m`.

```julia-repl
julia> m=[1 2 3;4 5 6;7 8 9]
3×3 Matrix{Int64}:
 1  2  3
 4  5  6
 7  8  9

julia> m^(Perm(1,2),Perm(2,3))
3×3 Matrix{Int64}:
 4  6  5
 1  3  2
 7  9  8
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L486-L505' class='documenter-source'>source</a><br>

<a id='Perms.restricted-Tuple{Perm, AbstractVector{<:Integer}}' href='#Perms.restricted-Tuple{Perm, AbstractVector{<:Integer}}'>#</a>
**`Perms.restricted`** &mdash; *Method*.



`restricted(a::Perm,l::AbstractVector{<:Integer})`

`l` should be a union of cycles of `p`; returns `p` restricted to `l`

```julia-repl
julia> restricted(Perm(1,2)*Perm(3,4),3:4)
(3,4)
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L660-L669' class='documenter-source'>source</a><br>

<a id='Perms.reflength-Tuple{Perm}' href='#Perms.reflength-Tuple{Perm}'>#</a>
**`Perms.reflength`** &mdash; *Method*.



`reflength(a::Perm)`

is   the  "reflection   length"  of   `a`,  that   is,  minimum  number  of transpositions of which `a` is the product


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L635-L640' class='documenter-source'>source</a><br>

<a id='Perms.mappingPerm' href='#Perms.mappingPerm'>#</a>
**`Perms.mappingPerm`** &mdash; *Function*.



`mappingPerm(a)`

given  a list  of positive  integers without  repetition `a`, this function finds  a permutation  `p` such  that `sort(a).^p==a`.  This can  be used to translate between arrangements and `Perm`s.

```julia-repl
julia> p=mappingPerm([6,7,5])
(5,6,7)

julia> (5:7).^p
3-element Vector{Int16}:
 6
 7
 5
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L675-L692' class='documenter-source'>source</a><br>


`mappingPerm(a,b)`

given two lists of positive integers without repetition `a` and `b`, this function finds a permutation `p` such that `a.^p==b`.

```julia-repl
julia> mappingPerm([1,2,5,3],[2,3,4,6])
(1,2,3,6,5,4)
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L700-L710' class='documenter-source'>source</a><br>

<a id='Perms.Perm_rowcol' href='#Perms.Perm_rowcol'>#</a>
**`Perms.Perm_rowcol`** &mdash; *Function*.



`Perm_rowcol(m1::AbstractMatrix, m2::AbstractMatrix)`

whether `m1` is conjugate to `m2` by row/col permutations.

`m1` and `m2` should be rectangular matrices of the same size. The function returns a pair of permutations `(p1,p2)` such that `m1^(p1,p2)==m2` if such permutations exist, `nothing` otherwise.

The entries of `m1` and `m2` must be sortable.

```julia-repl
julia> a=[1 1 1 -1 -1; 2 0 -2 0 0; 1 -1 1 -1 1; 1 1 1 1 1; 1 -1 1 1 -1]
5×5 Matrix{Int64}:
 1   1   1  -1  -1
 2   0  -2   0   0
 1  -1   1  -1   1
 1   1   1   1   1
 1  -1   1   1  -1

julia> b=[1 -1 -1 1 1; 1 1 -1 -1 1; 1 -1 1 -1 1; 2 0 0 0 -2; 1 1 1 1 1]
5×5 Matrix{Int64}:
 1  -1  -1   1   1
 1   1  -1  -1   1
 1  -1   1  -1   1
 2   0   0   0  -2
 1   1   1   1   1

julia> p1,p2=Perm_rowcol(a,b)
((1,2,4,5,3), (3,5,4))

julia> a^(p1,p2)==b
true
```


<a target='_blank' href='https://github.com/jmichel7/Perms.jl/blob/b69eaa34a12cb2532c01c153e4f98aaca2d58a09/src/Perms.jl#L719-L753' class='documenter-source'>source</a><br>

