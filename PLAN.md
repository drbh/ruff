# Plan

- Inspect `crates/ty_python_semantic/src/types/constraints.rs` to locate `ConstraintSet` construction points and BDD node creation, plus any callers that will need to propagate a new support field.
- Add a new `#[salsa::interned] Support` type wrapping `FxOrderSet<ConstrainedTypeVar<'db>>`, then extend `ConstraintSet` to store both `node` and `support` and update all constructors/operations (constrain/always/never/and/or/union/intersect/negate/exists/iff/etc.) to merge or preserve support using `OrderSet::union` semantics.
- Re-enable full BDD reduction by short-circuiting `Node::new` when `if_true == if_false`, and update comments/docs describing quasi-reduction to reflect full reduction plus separate support tracking.
- Audit constraint-set combining helpers to ensure support carries through (including any display/debug helpers) and update any tests/docs impacted by the new representation if needed.
