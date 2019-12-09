# GitHub ESLint runner

This action executes ESLint linter on specified javascript files without any previous action/build step or Docker required.

## Prerequisites

### ESLint
You must have the ESLint running locally for the action to execute. It will use the same rules as you do locally.
More info [on the ESLint getting started guide](https://eslint.org/docs/user-guide/getting-started#installation-and-usage)

## Usage

### main.workflow

Add any of the examples below to your workflow file in `.github/main.workflow`

This is the simplest example to get it running:
```
workflow "New workflow" {
  on = "push"
  resolves = ["ESLint"]
}

action "ESLint" {
  uses = "stefanoeb/eslint-action@master"
}
```

By default it will run ESLint through all the files in the project. But you can also specify a glob of files on the `args`, just like ESLint:
```
workflow "New workflow" {
  on = "push"
  resolves = ["ESLint"]
}

action "ESLint" {
  uses = "stefanoeb/eslint-action@master"
  args = "index.js src/**.js"
}
```

If there is no previous step installing the necessary modules, this action will execute a `yarn install` or `npm install` automatically.

### Alternative: lint.yml

```
name: Lint

on: pull_request

jobs:
  eslint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
        with:
          fetch-depth: 1
      - uses: actions/setup-node@v1
        with:
          node-version: 12
      - run: npm ci --no-audit --prefer-offline
      - uses: stefanoeb/eslint-action@1.0.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          args: './src/**/*.{js,ts,tsx} --max-warnings 0'
```

## License

The Dockerfile and associated scripts and documentation in this project are released under the [MIT License](LICENSE).
