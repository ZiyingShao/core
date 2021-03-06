# core

## Prepare

Download dependencies ([parsers](https://github.com/crimson-unicorn/parsers), [graphchi](https://github.com/crimson-unicorn/graphchi-cpp) and [modeling](https://github.com/crimson-unicorn/modeling)):
```
make prepare
```

It is possible to specify the dependencies version (if unspecified master branch is used by default):
```
make parsers-version=<branch or tag> graphchi-version=<branch or tag> modeling-version=<branch or tag> prepare
```

For example to use the `selective` branch/tag for the graphchi dependency:
```
make graphchi-version=selective prepare
```

## Download datasets

To download, for example the wget datasets:
```
make download_wget
```

## Clean

Clean everything:
```
make clean
```
