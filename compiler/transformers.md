See also, this guide which is probably better than anything you'll find below: https://github.com/madou/typescript-transformer-handbook

---

TypeScript `SourceFile`s are immutable.  Transformers must return a new SourceFile, not mutate the original.

Here's a very abstract example of a transformer's input and output ASTs.  These names are meaningless and do not represent real AST nodes.

The transformer wants to replace `biff` with `barry`.

Input:
```
root
  foo
    bar
  baz
    biff
```

Output:
```
newRoot
  foo
    bar
  newBaz
    barry
```

In the example, `root` cannot be mutated.  In order to replace `biff` with `barry`, `baz` needed to be replaced with a `newBaz`, which is a copy of `baz` with different children.
This necessitated creating `newRoot`, which is a copy of `root` with different children.  `foo` was reused because it does not need to be modified.

## Useful APIs

* `ts.getMutableClone`
* `ts.create*` node factories
  * all call `ts.createSynthesizedNode` under the hood
* `ts.visitEachChild` which takes care of cloning ancestor nodes as needed
