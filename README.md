# `tmp-gh-action-pr-context`

A quick playground to solve the problem of extracting the data from the `.context` of PR-related Github actions once and for all.

## PR1

Testing the initial action.

The hash of `PR1 commit1` is `876f123f51efe07e99c87209fe5f01692b37b099`.
The hash of `PR1 commit2` is `4d9540e555f92800d237d012d732f138f60397b8`.

From the [first action run](https://github.com/dkorolev/tmp-gh-action-pr-context/actions/runs/5938948010/job/16104386303):

```
cat c1.json | jq .eventName
"pull_request"

cat c1.json | jq .payload.number
1

cat c1.json | jq .payload.action
"opened"

cat c1.json | jq .payload.pull_request.head.sha
"876f123f51efe07e99c87209fe5f01692b37b099"
```

From the [second action run](https://github.com/dkorolev/tmp-gh-action-pr-context/actions/runs/5938957144/job/16104411074):

```
cat c2.json | jq .eventName
"pull_request"

cat c2.json | jq .payload.number
1

cat c2.json | jq .payload.action
"synchronize"

cat c2.json | jq .payload.after
"4d9540e555f92800d237d012d732f138f60397b8"
```

Lessons learned from PR1.

1. There are indeed differences between `"opened"` and `"synchronize"`.
2. The `HEAD` commit is totally broken, may be related to (3).
3. Need to upgrade the version of `actions/checkout@v2`, and need to properly do a shallow clone.

# PR2

Testing with `v3` of `actions/checkout`.

The hash of `PR2 commit1` is `2d7a6e2c4970be5cd68f9d9776b46a2e688c0ad8`.

From the [first action run](https://github.com/dkorolev/tmp-gh-action-pr-context/actions/runs/5939156679/job/16104991679):

```
cat c1.json | jq .eventName
"pull_request"

cat c1.json | jq .payload.number
2

cat c1.json | jq .payload.action
"opened"

cat c1.json | jq .payload.pull_request.head.sha
"2d7a6e2c4970be5cd68f9d9776b46a2e688c0ad8"
```

Lessons learned from PR2.

1. Need to change `s/base/head/` for `"opened"`.
