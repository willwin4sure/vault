For the purposes of this class, we say that an action is ==atomic== if it happens **completely or not at all**. 

> [!idea] The golden rule of atomicity
> Never modify the only copy!

^1d0cb6



> [!idea] Write-ahead-log protocol
> Log the update *before* installing it.

This is a special case of the [[Atomicity#^1d0cb6|golden rule]] of atomicity.

