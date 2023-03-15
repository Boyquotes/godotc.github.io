
`allowance[A][B]`  means the amount that B can transfer  from A.

`transferFrom` can be called by B to transfer from A to B. But it must be verified by the allowance, or it will throw error.

`transfer` no need allowance