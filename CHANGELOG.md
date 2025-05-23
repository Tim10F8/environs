# Changelog

## 14.2.0 (2025-05-22)

Features:

- Add `key_value_delimiter` argument to `env.dict` ([#419](https://github.com/sloria/environs/issues/419)).
  Thanks [jerr0328](https://github.com/jerr0328) for the suggestion and PR.

## 14.1.1 (2025-02-10)

Bug fixes:

- Typing: Fix typing for `env.list` and `env.dict` to properly handle
  `default` and `subcast` arguments ([#406](https://github.com/sloria/environs/issues/406)).
  Thanks [lucas-bremond](https://github.com/lucas-bremond) for the PR.
- Add `env` to `__all__` ([#396](https://github.com/sloria/environs/issues/396)).
  Thanks [daveflr](https://github.com/daveflr) for reporting.

Changes:

- _Backwards-incompatible_: `recurse`, `verbose`, `override`,
  and `return_path` parameters to `Env.read_env` are now keyword-only.
- _Backwards-incompatible_: The `required` argument to parser methods
  is removed. Call a parser method without a default value to make it required.

## 14.1.0 (2025-01-10)

Features:

- Add `prefix` parameter to `Env` constructor ([#384](https://github.com/sloria/environs/issues/384)).
  Thanks [arthurc0102](https://github.com/sloria/environs/issues/384)
  for the suggestion.
- Re-export `validate` module from marshmallow ([#385](https://github.com/sloria/environs/issues/385)).

Bug fixes:

- Typing: Fix typing for `Env.enum` to allow `by_value` to be passed ([#386](https://github.com/sloria/environs/issues/386)).

## 14.0.0 (2025-01-07)

Features:

- Add `env` singleton ([#194]).
  Thanks [AndBondStyle](https://github.com/AndBondStyle) for the suggestion.

```python
from environs import env
```

Changes:

- `default` values are expected to be their in their deserialized form.
  _Backwards-incompatible_: Passing serialized values to `default`
  is no longer supported.

```python
from datetime import date, timedelta
import environs

# DO
enable_login = env.bool("ENABLE_LOGIN", True)
ttl = env.timedelta("TTL", default=timedelta(seconds=600))
release_date = env.date("RELEASE", date(2025, 1, 7))
numbers = env.list("FOO", [1.0, 2.0, 3.0], subcast=float)

# DON'T
enable_login = env.bool("ENABLE_LOGIN", "true")
ttl = env.timedelta("TTL", default=600)
release_date = env.date("RELEASE", "2025-01-07")
numbers = env.list("NUMBERS", "1,2,42", subcast=float)
```

The exceptions to this rule are the Django-related functions, which
accept string defaults.

```python
DATABASE_URL = env.dj_db_url("DATABASE_URL", default="postgresql://localhost:5432/mydb")
```

This fixes [#297](https://github.com/sloria/environs/issues/297)
and [#270](https://github.com/sloria/environs/issues/270).

## 13.0.0 (2025-01-07)

Features:

- Typing: Add type hints for parser methods ([#367](https://github.com/sloria/environs/issues/367)).
  Thanks [OkeyDev](https://github/OkeyDev) for the PR.
- Add `by_value` param to `Env.enum`. Pass `by_value=True` to parse Enum by values instead of names.

Other changes:

- Drop support for marshmallow<3.18.0.
- _Backwards-incompatible_: Remove `load_default` param from parser methods. Use `default` instead.
- _Backwards-incompatible_: Rename `Env.enum`'s `type` param to `enum`.
- _Backwards-incompatible_: Remove `ignore_case` param from `Env.enum`.
- _Backwards-incompatible_: Remove
  `allow_none`, `error_messages`, and `metadata` params from parser methods.

## 12.0.0 (2025-01-06)

Features:

- `Env.enum` allows passing an `Enum` as `default`.

Other changes:

- Support marshmallow 4.
- Support Python 3.9-3.13.
- _Backwards-incompatible_: Make `Field` classes private. Users should
  not need to access these.

## 11.2.1 (2024-11-20)

Bug fixes:

- Fix passing non-string default values to `Env.timedelta` ([#369](https://github.com/sloria/environs/pull/369)).
  Thanks [tcleonard](https://github.com/tcleonard) for reporting and
  thanks [ddelange](https://github.com/ddelange) for the PR.
- Fix `Env.read_env(recurse=True)` so that it returns as soon as a `.env`
  file is found ([#370](https://github.com/sloria/environs/pull/370)).
  Thanks [senese](https://github.com/senese) for the PR.

## 11.2.0 (2024-11-14)

Features:

- `Env.timedelta` can parse [GEP-2257](https://gateway-api.sigs.k8s.io/geps/gep-2257/)
  duration strings ([#366](https://github.com/sloria/environs/pull/366)).
  Thanks [ddelange](https://github.com/ddelange) for the PR.

## 11.1.0 (2024-11-11)

Features:

- Add `return_path` argument to `Env.read_env` to return the path of the
  parsed file ([#362](https://github.com/sloria/environs/issues/362)).
  Thanks [senese](https://github.com/senese) for the suggestion and PR.
  NOTE: This is added as a backwards-compatible change that is scheduled to be
  removed in 12.0.0. The path will stored as an attribute of `Env`.

## 11.0.0 (2024-03-04)

Fixes:

- Prevent `DeprecationWarning` getting raised by marshmallow 3.21.0 (:issue:`333`).

Other changes:

- Remove `subcast_key` argument of `env.dict`. Use `subcast_keys` instead.
- Bump minimum required marshmallow version to 3.13.0.
- Remove `missing` argument from `Env` methods. Use `default` instead.
- Remove `subcast` argument from `Env` methods that don't use it.
- _Backwards-incompatible_: Remove `environs.__version__` variable.
  Use `importlib.metadata.version("environs")` instead.

## 10.3.0 (2024-01-11)

Features:

- `Env.__repr__` and `Env.__str__` no longer display environment variables, to prevent accidental exfiltration of sensitive data ([#292](https://github.com/sloria/environs/issues/292)).
  Thanks [mro-rhansen2](https://github.com/mro-rhansen2) for the suggestion.

Bug fixes:

- Fix passing `None` as default to `env.list` ([#298](https://github.com/sloria/environs/issues/298)).
  Thanks [lucas-bremond](https://github.com/lucas-bremond) for reporting.

## 10.2.0 (2024-01-09)

Features:

- `env.read_env` returns `False` if file not found ([#294](https://github.com/sloria/environs/issues/294)).
  Thanks [rptaylor](https://github.com/rptaylor) for the suggestion.

## 10.1.0 (2024-01-08)

Features:

- Allow `default` for `env.json` to be a `dict` or `list` ([#240](https://github.com/sloria/environs/pull/240)).
  Thanks [tomgrin10](https://github.com/tomgrin10) and [bvanelli](https://github.com/bvanelli) for the PRs.

Other changes:

- Test against Python 3.12.

## 10.0.0 (2023-12-15)

Features:

- Add `delimiter` argument to `env.dict` ([#277](https://github.com/sloria/environs/pull/277)). Thanks [gregoiredx](https://github.com/gregoiredx) for the PR.

Other changes:

- _Backwards-incompatible_: Drop support for EOL Python 3.6 and 3.7.
- Test against Python 3.11.
- Migrate to setup.cfg. Thanks [deronnax](https://github.com/deronnax) for the PR.

## 9.5.0 (2022-01-30)

Features:

- Allow callables or custom marshmallow fields to be passed to `subcast`, `subcast_keys`, and `subcast_values`. ([#241](https://github.com/sloria/environs/pull/241)).
  Thanks [bvanelli](https://github.com/bvanelli) for the PR.

## 9.4.0 (2022-01-04)

Bug fixes:

- Address warning about passing metadata in newer versions of marshmallow.

Other changes:

- Test against Python 3.10.

## 9.3.5 (2021-11-14)

Bug fixes:

- Fix `env.url` and other parsers that take field keyword arguments ([#227](https://github.com/sloria/environs/issues/227)).
  Thanks [KochankovID](https://github.com/KochankovID) for the PR.

## 9.3.4 (2021-10-03)

Bug fixes:

- Allow default for `env.path` to be a `pathlib.Path` ([#228](https://github.com/sloria/environs/issues/228)).
  Thanks [KochankovID](https://github.com/KochankovID) for the PR.

## 9.3.3 (2021-08-08)

Bug fixes:

- Fix compatibility with marshmallow>=3.13.0
  so that no DeprecationWarnings are raised ([#224](https://github.com/sloria/environs/issues/224)).

## 9.3.2 (2021-03-28)

Bug fixes:

- Handle JSON decoding errors when using `env.json` ([#212](https://github.com/sloria/environs/pull/212)).
  Thanks [bvanelli](https://github.com/bvanelli) for the PR.

## 9.3.1 (2021-02-07)

Bug fixes:

- Apply variable expansion to default values ([#204](https://github.com/sloria/environs/pull/204)).
  Thanks [rjcohn](https://github.com/rjcohn) for the PR.

## 9.3.0 (2020-12-26)

Deprecations:

- Rename `subcast_key` argument of `env.dict` to `subcast_keys`
  for consistency with `subcast_values`. `subcast_key` is deprecated.

## 9.2.0 (2020-11-07)

Features:

- Add time parser ([#191](https://github.com/sloria/environs/issues/191)).

## 9.1.0 (2020-11-06)

Features:

- Add `enum` parser ([#185](https://github.com/sloria/environs/pull/185)).
- Add `delimiter` param to `env.list`
  ([#184](https://github.com/sloria/environs/pull/184)).

Thanks [tomgrin10](https://github.com/tomgrin10) for the PRs.

Bug fixes:

- Loosen `ParserMethod` typing ([#186 (comment)](https://github.com/sloria/environs/issues/186#issuecomment-723163520)).
  Thanks [hukkinj1](https://github.com/hukkinj1) for the PR.

Other changes:

- When using deferred validation (`eager=False`), parser methods return `None`
  for missing or invalid values.
  _Note_: This may break code that depended on parser methods returning `marshmallow.missing`,
  but this behavior was not documented nor tested and therefore is not considered public API.

## 9.0.0 (2020-10-31)

- _Backwards-incompatible_: Rename `subcast` param of `env.dict` to `subcast_values` for consistency with `subcast_keys`.
- _Backwards-incompatible_: Remove variable proxying. Use variable expansion instead (see 8.1.0 release notes below)
  ([#175](https://github.com/sloria/environs/issues/175)).
- _Backwards-incompatible_: Drop support for marshmallow 2 and Python 3.5,
  which are both EOL ([#174](https://github.com/sloria/environs/issues/174)).

## 8.1.0 (2020-10-31)

Features:

- Add support for variable expansion, e.g. `MY_VAR=${MY_OTHER_VAR:-mydefault}` ([#164](https://github.com/sloria/environs/issues/164)).
  Thanks [gnarvaja](https://github.com/gnarvaja) for the PR.

Deprecations:

- Variable proxying using the `{{VAR}}` syntax is deprecated
  and will be removed in environs 9.0.0.
  Use variable expansion using `${VAR}` instead.

```bash
# Before
export MAILGUN_LOGIN=sloria
export SMTP_LOGIN={{MAILGUN_LOGIN}}

# After
export MAILGUN_LOGIN=sloria
export SMTP_LOGIN=${MAILGUN_LOGIN}
```

```python
from environs import Env

env = Env(expand_vars=True)

SMTP_LOGIN = env.str("SMTP_LOGIN")  # => 'sloria'
```

Bug fixes:

- Fix deferred validation behavior for `dj_db_url`, `dj_email_url`, `dj_cache_url`,
  and custom parsers ([#121](https://github.com/sloria/environs/issues/121)).
  Thanks [hukkinj1](https://github.com/hukkinj1) for reporting.

Other changes:

- Test against Python 3.9.
- Remove usage of implicit `typing.Optional` ([171](https://github.com/sloria/environs/issues/171)).

## 8.0.0 (2020-05-27)

Bug fixes:

- Fix behavior of recurse=True when custom filepath is passed to `env.read_env`
  ([#100](https://github.com/sloria/environs/issues/100)). Thanks [ribeaud](https://github.com/ribeaud) and [timoklimmer](https://github.com/sloria/environs/pull/157) for the help.

Other changes:

- _Backwards-incompatible_: As a result of the above fix, passing a directory to `env.read_env` is no longer allowed and will raise a `ValueError`.
  Only file paths or file names should be passed.

## 7.4.0 (2020-04-18)

- Add `subcast_key` argument to `env.dict` ([#151](https://github.com/sloria/environs/issues/151)).
  Thanks [AugPro](https://github.com/AugPro) for the suggestion and PR.

## 7.3.1 (2020-03-22)

- Fix error when parsing empty list with subcast
  [#137](https://github.com/sloria/environs/issues/137).
  Thanks [sabdouni] for the catch and patch.

## 7.3.0 (2020-03-01)

- `log_level` accepts lower-cased log level names and rejects invalid
  names ([#138](https://github.com/sloria/environs/pull/138)).
  Thanks [gnarvaja](https://github.com/gnarvaja) for the PR.

## 7.2.0 (2020-02-09)

- Add `dj_cache_url` for caching Django cache URLs (requires installing with `[django]`)
  ([#126](https://github.com/sloria/environs/issues/126)).
  Thanks [epicserve](https://github.com/epicserve) for the suggestion and PR.

## 7.1.0 (2019-12-07)

- Improve typings and run mypy with dependencies type annotations ([#115](https://github.com/sloria/environs/pull/115)).
- Distribute types per PEP 561 ([#116](https://github.com/sloria/environs/pull/116)).

Thanks [hukkinj1](https://github.com/hukkinj1) for the PRs.

## 7.0.0 (2019-12-02)

- _Backwards-incompatible_: Remove `stream` argument from `read_env`,
  since it had no effect ([#114](https://github.com/sloria/environs/pull/114)).
- _Backwards-incompatible_: `Env.read_env` consistently returns `None`
  ([#111](https://github.com/sloria/environs/pull/111)).
- Remove unnecessary `__str__` definition ([#112](https://github.com/sloria/environs/pull/112)).

Thanks [hukkinj1](https://github.com/hukkinj1) for the PRs.

## 6.1.0 (2019-11-03)

Features:

- Add deferred validation via the `eager` parameter and `env.seal()` ([#56](https://github.com/sloria/environs/issues/56)).
  Thanks [robertlagrant](https://github.com/robertlagrant) for the suggestion.

Other changes:

- Test against Python 3.8 ([#108](https://github.com/sloria/environs/pull/108)).

## 6.0.0 (2019-09-22)

Features:

- Default parser methods are now defined as bound methods.
  This enables static analysis features, e.g. autocomplete ([#103](https://github.com/sloria/environs/issues/103)).
  Thanks [rugleb](https://github.com/rugleb) for the suggestion.
  _Backwards-incompatible_: As a result of this change, adding a parser name that is the same as an existing method
  will result in an error being raised.

```python
import environs

env = environs.Env()


# Below conflicts with built-in `url` method.
# In <6.0.0, this would override the built-in method.
# In >=6.0.0, this raises an error:
#   environs.ParserConflictError: Env already has a method with name 'url'. Use a different name.
@env.parser_for("url")
def https_url(value):
    return "https://" + value
```

Bug fixes:

- Fix error message for prefixed variables ([#102](https://github.com/sloria/environs/issues/102)).
  Thanks [AGeekInside](https://github.com/AGeekInside) for reporting.

Other changes:

- _Backwards-incompatible_: Rename `Env.__parser_map__` to `Env.__custom_parsers__`.

## 5.2.1 (2019-08-08)

Bug fixes:

- Fix behavior when recursively searching for a specified file
  ([#96](https://github.com/sloria/environs/issues/96)).
  Thanks [ribeaud](https://github.com/ribeaud) for the catch and patch.

## 5.2.0 (2019-07-19)

Changes:

- Improve typings.

## 5.1.0 (2019-07-13)

Features:

- Add `env.log_level` ([#7](https://github.com/sloria/environs/issues/7)).
- Use `raise from` to improve tracebacks.

Other changes:

- Improve typings.

## 5.0.0 (2019-07-06)

Features:

- Add `env.path` ([#81](https://github.com/sloria/environs/issues/81)).
  Thanks [umrashrf](https://github.com/umrashrf) for the suggestion.
- Add type annotations.

Other changes:

- _Backwards-incompatible_: Drop support for Python 2. If you use Python 2,
  you will need to use version 4.2.0 or older.

## 4.2.0 (2019-06-01)

- Minor optimization.

Bug fixes:

- Reset prefix when an exception is raised within an `env.prefixed()`
  context ([#78](https://github.com/sloria/environs/issues/78)).
  Thanks [rcuza](https://github.com/rcuza) for the catch and patch.

## 4.1.3 (2019-05-15)

Bug fixes:

- Fix behavior when passing a `dict` value as the default
  to `env.dict` ([#76](https://github.com/sloria/environs/pull/76)).
  Thanks [c-w](https://github.com/c-w) for the PR.

Support:

- Document how to read a specific file with `env.read_env`
  ([#66](https://github.com/sloria/environs/issues/66)).
  Thanks [nvtkaszpir](https://github.com/nvtkaszpir) and
  [c-w](https://github.com/c-w).

## 4.1.2 (2019-05-05)

Bug fixes:

- Fix compatibility with marshmallow 3.0.0>=rc6.

## 4.1.1 (2019-05-04)

Bug fixes:

- Fix accessing proxied envvars when using `env.prefixed`
  ([#72](https://github.com/sloria/environs/issues/72)).
  Thanks [Kamforka](https://github.com/Kamforka) for the catch and patch.
- Fix behavior when an envvar is explicitly set to an empty string
  ([#71](https://github.com/sloria/environs/issues/71)).
  Thanks [twosigmajab](https://github.com/twosigmajab) for reporting
  and thanks [hvtuananh](https://github.com/hvtuananh) for the PR.

## 4.1.0 (2018-12-10)

- `EnvError` subclasses `ValueError` ([#50](https://github.com/sloria/environs/pull/50)).
  Thanks [alexpirine](https://github.com/alexpirine).
- Test against Python 3.7.

## 4.0.0 (2018-08-06)

- Use python-dotenv for parsing .env files. `Env.read_env` behaves
  mostly the same except that a warning isn't raised by default if a
  .env file isn\'t found. Pass `verbose=True` to raise a warning.

## 3.0.0 (2018-08-05)

Features:

- _Backwards-incompatible_: `Env.read_env` raises a warning instead of
  an error when `.env` isn\'t found
  ([#10](https://github.com/sloria/environs/issues/10)). Thanks
  [lachlancooper](https://github.com/lachlancooper) for the
  suggestion.
- Add optional Django support. Install using
  `pip install environs[django]`, which enables `env.dj_db_url` and
  `env.dj_email_url`.

## 2.1.1 (2018-05-21)

Features:

- Fix compatibility with marshmallow 3 beta.

## 2.1.0 (2018-01-25)

Features:

- Add recurse parameter to Env.read_env
  ([#9](https://github.com/sloria/environs/pull/9)). Thanks
  [gthank](https://github.com/gthank) for the PR.

## 2.0.0 (2018-01-02)

Features:

- Add support for nested prefixes
  ([#8](https://github.com/sloria/environs/pull/8)). Thanks
  [gvialetto](https://github.com/gvialetto) for the PR.

Other changes:

- _Backwards-incompatible_: Drop support for Python 3.3 and 3.4.

## 1.2.0 (2017-01-12)

Features:

- Add `url` parser that returns a `urllib.parse.ParseResult`
  ([#6](https://github.com/sloria/environs/issues/6)). Thanks
  [IlyaSemenov](https://github.com/IlyaSemenov) for the suggestion.

Bug fixes:

- Every instance of `Env` gets its own parser map, so calling
  `env.parser_for` for one instance doesn\'t affect other instances.

## 1.1.0 (2016-05-01)

- Add `Env.read_env` method for reading `.env` files.

## 1.0.0 (2016-04-30)

- Support for proxied variables
  ([#2](https://github.com/sloria/environs/issues/2)).
- _Backwards-incompatible_: Remove `env.get` method. Use `env()`
  instead.
- Document how to read `.env` files
  ([#1](https://github.com/sloria/environs/issues/1)).

## 0.1.0 (2016-04-25)

- First PyPI release.
