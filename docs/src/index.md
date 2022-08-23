# Permutations
```@contents
Depth=3
```

```@docs
Perms
Perm
Perm(::Integer...)
Perm(::AbstractMatrix{<:Integer})
Perm(::AbstractVector, ::AbstractVector)
Perm(::AbstractMatrix, ::AbstractMatrix)
@perm_str
largest_moved_point(::Perm)
smallest_moved_point
Base.:^(::AbstractVector,::Perm) 
sortPerm
randPerm
orbit(::Perm,::Integer)
orbits(::Perm)
Perms.order
cycles(::Perm)
cycletype(::Perm)
support
sign
Base.Matrix(::Perm,n)
Base.:^(::AbstractMatrix,::Perm)
Base.:^(::AbstractMatrix,::Tuple{Perm,Perm})
restricted(::Perm,::AbstractVector{<:Integer})
reflength(::Perm)
mappingPerm
Perm_rowcol
```
