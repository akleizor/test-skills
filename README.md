# test-skills

Two agent skills for writing tests and keeping a suite fast, forked from
[mtschoen/skills-working-method](https://github.com/mtschoen/skills-working-method)
with the other skills removed and local lessons added.

| Skill | Use it when |
| --- | --- |
| [`writing-tests`](writing-tests/) | About to write, change, merge or delete a test, fixture or test double |
| [`fast-tests`](fast-tests/) | A test run is slow enough to block the work |

Local additions (in `fast-tests/references/`): worker count on hybrid CPUs,
unittest module cleanups under pytest, per-element browser round trips, a
`subTest` per source line, and durable-storage settings in test databases.

Upstream changes: `gh repo sync akleizor/test-skills`, then pull.

MIT, see `LICENSE`.
