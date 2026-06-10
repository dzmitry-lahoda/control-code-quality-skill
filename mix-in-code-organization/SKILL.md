# Code Organization

Use feature modules for large domain features.

## Goal

Keep central dispatch files small and stable. Large root enum dispatch should remain in root files, but branch implementations, domain types, math, algorithms, and helper logic should live under the feature module.

This reduces merge conflicts in high-churn files such with high dispatch level. It also makes feature work easier to review and easier for coding agents to load into context.

Shared enums and cross-feature types can stay in shared/root files. Move them only when ownership is clear.

## Core Ideas

- Central dispatch stays central. Root files own enum matching and orchestration.
- Feature implementation moves into feature folders. A dispatch branch should usually delegate to a feature function instead of carrying the implementation inline.
- Module layout should express visibility. Public feature structs and entrypoints stay close to the module root; private helpers and narrow implementation details move deeper.

## Pattern


Root files should usually only match the enum variant and call into feature code.


The root owns dispatch. The feature module owns implementation details.

## Guidelines

- Put feature-specific types near the feature, 
- Keep public feature API near the module root or in clearly named files.
- Move private helpers deeper into nested submodules, or keep them private to the file that uses them.
- Do not force a rigid layout; split files only where it reduces conflicts and context size.
- Small feature APIs can stay in one file, instead of being split too early.
- Existing code does not need to be moved all at once. New or heavily updated features should follow this pattern when practical.
- Multi feature used to be in shared files