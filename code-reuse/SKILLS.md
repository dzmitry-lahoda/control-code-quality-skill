# Code Reuse Review Prompt

Search this PR for code reuse opportunities. Include production code and reusable test infrastructure such as invariants, assertions, builders, and helpers. Do not scan docs, examples, or one-off test cases.

Focus on duplicated or near-duplicated code within the PR diff and between PR code and existing code. Weight findings higher when the duplicated code is larger, more procedural, located closer together, repeated more times, and each reused input parameters has a distinct type.

Avoid suggesting reuse that is unrelated to newly added code. Avoid reuse that replaces named parameters, fields, or builders with unnamed methods/functions that take many same-typed inputs. Avoid closures; prefer functions or methods.

Deweight suggestions when the changes needed for reuse are larger than the saved code size. Deweight suggestions that require creating new structs. Avoid reuse when it requires returning empty/default values or writing no-op functions/methods.

Return only actionable reuse suggestions with file/line references and a short explanation of what helper, function, method, or abstraction should be extracted. If suggestions are applied, report lines added and removed.
