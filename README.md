# ci

Ready-made pipelines for continuous integration of taiga projects

---

### Auto approve action

```yml
name: Auto approve
on: pull_request

jobs:
  automated-approve-release-pull-request:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    if: contains(github.head_ref, 'release/')
    steps:
      - uses: actions/checkout@v3
      - uses: taiga-family/actions/two-approve@1.3.0
        with:
          token1: ${{ secrets.APPROVER1_TOKEN }}
          token2: ${{ secrets.APPROVER2_TOKEN }}
```

### Global variables action

```yml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: taiga-family/ci/actions/variables@1.3.0
```

### Node.js action

```yml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: taiga-family/ci/actions/setup-node@1.3.0
```
