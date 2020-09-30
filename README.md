# Study github-go

## File Structure

It is an API client library for accessing [GitHub REST API v3](https://docs.github.com/en/rest), and it's called `github-go`

### Community friendly

* [AUTHORS](https://github.com/google/go-github/blob/master/AUTHORS) for adding people who contribute
* [CONTRIBUTING.md](https://github.com/google/go-github/blob/master/CONTRIBUTING.md) and some rules for code quality
* [LICENSE](https://github.com/google/go-github/blob/master/LICENSE) it has BSD license
* [README.md](https://github.com/google/go-github/blob/master/README.md) obviously

### Automation and testing

#### Github Actions

They use GitHub actions [tests.yml](https://github.com/google/go-github/blob/master/.github/workflows/tests.yml) which is doing the following:

* Setup a specific version of Go on two platforms (Ubuntu and Windows) using [setup-go](https://github.com/actions/setup-go)
* Caches dependencies and build outputs to [improve work flow execution time[(https://docs.github.com/en/free-pro-team@latest/actions/guides/caching-dependencies-to-speed-up-workflows)] using [cache](https://github.com/actions/cache)
* Runs `go fmt`
* Ensures `go generate` produces a zero diff
* Runs `go vet`
* Runs `go test` with coverage
* Build but not run integration tests (since they hit live GitHub API)
* Run scrape tests
* Upload the results to CodeCov using [CodeCov-Action](https://github.com/codecov/codecov-action)

#### Badges

* It uses [CLI best practices](https://bestpractices.coreinfrastructure.org/projects/796)
* It has [godoc reference](https://pkg.go.dev/github.com/google/go-github/v32/github)
* A [tests passing](https://github.com/google/go-github/workflows/tests/badge.svg)
* A [codecov score](https://codecov.io/gh/google/go-github)

### 5 folders

#### github folder

The `github` folder is named after the API this client is made for that is mapping the [API Reference](https://docs.github.com/en/free-pro-team@latest/rest/reference) document.

For example there there are 30 services:

* [Actions](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions) - [actions.go](https://github.com/google/go-github/blob/master/github/actions.go)
* [Activity](https://docs.github.com/en/free-pro-team@latest/rest/reference/activity) - [activity.go](https://github.com/google/go-github/blob/master/github/activity.go)
* [Apps](https://docs.github.com/en/free-pro-team@latest/rest/reference/apps) - [apps.go](https://github.com/google/go-github/blob/master/github/apps.go)
* [Billing](https://docs.github.com/en/free-pro-team@latest/rest/reference/billing) - nope
* [Checks](https://docs.github.com/en/free-pro-team@latest/rest/reference/checks) - [checks.go](https://github.com/google/go-github/blob/master/github/checks.go)
* [Codes of conduct](https://docs.github.com/en/free-pro-team@latest/rest/reference/codes-of-conduct) - nope
* [Code Scanning](https://docs.github.com/en/free-pro-team@latest/rest/reference/code-scanning) - [scanning.go](https://github.com/google/go-github/blob/master/github/code-scanning.go)
* and so on ....

Now, for every single on of those services, there are multiple things you can do. For example, for [Actions](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions) you can do stuff with:

* [Artifacts](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#artifacts) - [actions_artifacts.go](https://github.com/google/go-github/blob/master/github/actions_artifacts.go)
* [Secrets](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#secrets) - [actions_secrets.go](https://github.com/google/go-github/blob/master/github/actions_secrets.go)
* [Self-hosted runners](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#self-hosted-runners) - [actions_runners](https://github.com/google/go-github/blob/master/github/actions_runners.go)
* and so on ...

Every file has an equivalent `_test.go` file. For example:

* [actions_artifacts.go](https://github.com/google/go-github/blob/master/github/actions_artifacts.go) - [actions_artifacts_test.go](https://github.com/google/go-github/blob/master/github/actions_artifacts_test.go)
* [actions_runners.go](https://github.com/google/go-github/blob/master/github/actions_runners.go) - [action_runners_test.go](https://github.com/google/go-github/blob/master/github/actions_runners_test.go)
* and so on ...

All those files are part of the the `package github` (tests as well).

The actual client itself it written inside [github.go](https://github.com/google/go-github/blob/master/github/github.go) and it has its own test file as well [github_test.go](https://github.com/google/go-github/blob/master/github/github_test.go).

It has an [examlles_test.go](https://github.com/google/go-github/blob/master/github/examples_test.go) files which are examples in `godoc`.


#### test folder

Here there is a [README.md](https://github.com/google/go-github/blob/master/test/README.md) with the information on how to run the tests.
These tests call the real Github API, so they do not run by the CI.
They are triggered manually by users from their laptops.

There are two sub-folders here:

* Fields with [fields.go](https://github.com/google/go-github/blob/master/test/fields/fields.go) that is part of the `main package`
* Integration with a bunch of tests e.g. [users_test.go](https://github.com/google/go-github/blob/master/test/integration/users_test.go) and they are all part of the `integration package`


#### update-urls folder

A bunch of scripts that update links within this project.
These links are usually at the comment sections. See [this PR](https://github.com/google/go-github/pull/1621/files)


#### scrape folder

This is an alternative way of interacting with GitHub using screen scraping.
It is intended to be used as a supplement to the standard go-github library to access data that is not currently exposed by either the official REST or GraphQL APIs.
The client is written at [scrape.go](https://github.com/google/go-github/blob/master/scrape/scrape.go) and it part of the `scrape package`

#### example folder

This is full of [examples](https://github.com/google/go-github/tree/master/example).
It has a folder per example and inside there is a `main.go`. e.g. for how to interact with [commitpr](https://github.com/google/go-github/tree/master/example/commitpr) there is just one `main.go` file showing you how to do that.