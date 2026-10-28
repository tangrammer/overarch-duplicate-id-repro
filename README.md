# Overarch: a duplicate id empties the id index

Overarch 0.43.0, its own CLI, nothing else.

```sh
clojure -M -m org.soulspace.overarch.adapter.ui.cli -m models
clojure -M -m org.soulspace.overarch.adapter.ui.cli -m models-without-duplicate
```

`models/repro.edn` defines `:repro/a`, `:repro/b`, a container `:repro/twice`
written twice with different names, and a relation from `:repro/a` to
`:repro/b`. The duplicate is reported, and then the relation's ends, both
defined, are reported unresolved:

```
{:build-problems ({:type :duplicate-id, :element {:el :container, :id :repro/twice, :name "Twice, renamed"}, :parent nil}),
 :unresolved-refs-in-views (),
 :unresolved-refs-in-relations
 ({:unresolved-ref :repro/a, :parent :repro/a-uses-b}
  {:unresolved-ref :repro/b, :parent :repro/a-uses-b})}
```

`models-without-duplicate` is the same model without the second `:repro/twice`:
no problems.
