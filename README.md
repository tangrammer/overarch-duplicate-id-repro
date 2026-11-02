# Overarch: a duplicate id empties the id index

**Fixed in Overarch 0.44.0** ([#52](https://github.com/soulspace-org/overarch/issues/52)).
On 0.44.0 the same model reports the duplicate and nothing else:

```sh
clojure -Sdeps '{:deps {org.soulspace.clj/overarch {:mvn/version "0.44.0"}}}' \
  -M -m org.soulspace.overarch.adapter.ui.cli -m models
```

```
{:build-problems
 ({:type :duplicate-id,
   :element {:el :container, :id :repro/twice, :name "Twice, renamed"},
   :parent nil}),
 :unresolved-refs-in-views (),
 :unresolved-refs-in-relations ()}
```

The rest of this README is the report as written against 0.43.0.

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
