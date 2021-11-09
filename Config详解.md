### 描述

npm gets its configuration values from the following sources, sorted by priority:

#### Command Line Flags

Putting `--foo bar` on the command line sets the `foo` configuration parameter to `"bar"`. A `--` argument tells the cli parser to stop reading flags. Using `--flag` without 指定 any value will set the value to `true`

Example: `--flag1 --flag2` will set both configuration parameters to `true`, while `--flag1 --flag2 bar` will set `flag1` to `true`, and `flag2` to `bar`. Finally, `--flag1 --flag2 -- bar` will set both configuration parameters to `true`, and the `bar` is taken as a command argument

#### Environment Variables

Any environment variables that start with `npm_config_` will be interpreted as a configuration parameter. For example, putting `npm_config_foo=bar` in your environment will set the `foo` configuration parameter to `bar`. Any environment configurations that are not given a value will be given the value of `true`.Config values are case-insensitive, so `NPM_CONFIG_FOO=bar` will work the same.However, please note that inside [`scripts`](https://docs.npmjs.com/cli/v7/using-npm/scripts) npm will set its own environment variables and Node will 偏向喜欢 those lowercase versions over any uppercase ones that you might set.

#### npmrc Files

- per-project configuration file (`/path/to/my/project/.npmrc`)
- per-user configuration file (defaults to `$HOME/.npmrc`; configurable via CLI option `--userconfig` or environment variable `$NPM_CONFIG_USERCONFIG`)
- global configuration file (defaults to `$PREFIX/etc/npmrc`; configurable via CLI option `--globalconfig` or environment variable `$NPM_CONFIG_GLOBALCONFIG`)
- npm's 内置 configuration file (`/path/to/npm/npmrc`)

#### Default Configs

Run `npm config ls -l` to see a set of configuration parameters that are internal to npm, and are defaults if nothing else is specified.
