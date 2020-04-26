TypeScript `SourceFile`s are immutable.  Transformers must return a new SourceFile, not mutate the original.

Here's a very abstract example of a transformer's input and output ASTs.  These names are meaningless and do not represent real AST nodes.

The transformer wants to replace `biff` with `barry`.

```
root
  foo
    bar
  baz
    biff
```

```
newRoot
  foo
    bar
  newBaz
    barry
```

In the example, `root` cannot be mutated.  In order to replace `biff` with `barry`, `baz` needed to be replaced with a `newBaz`, which is a copy of `baz` with different children.
This necessitated creating `newRoot`, which is a copy of `root` with different children.  `foo` was reused because it does not need to be modified.
