## What the Action does
This action sets up the PHP environment on the Ubuntu machine. It validates and caches [Composer](https://getcomposer.org/) packages. In the end, it runs a code coverage tool by [Codecov](https://about.codecov.io/). The package is then available on the [Packagist](https://packagist.org/) repository.

## Notes
- It's recommended to use a Linux machine for building - `jobs.<name>.runs-on` property.
- PHP versions are specified in `strategy.matrix.destination` property.
- The action uses 3rd party [shivammathur/setup-php@v2](https://github.com/shivammathur/setup-php) action for setting PHP environment.
- Package on Packagist will be automatically crawled periodically. You just have to make sure you keep the composer.json file up to date - for more info take a look at the [Packagist documentation](https://packagist.org/).
- Version of the package on Packagist is automatically fetched from the [tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging).

## [GitHub Action example](https://github.com/kontent-ai/kontent-ai-delivery-sdk-php/blob/master/.github/workflows/integrate.yml)
- PHP project using Composer and Codecov
```yaml
name: Build & Test & Report

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:        
        php-versions: ['7.0', '7.1', '7.2', '7.3']
        phpunit-versions: ['latest']
    steps:
    - uses: actions/checkout@v2
    - name: Setup PHP
      uses: shivammathur/setup-php@v2
      with:
        php-version: ${{ matrix.php-versions }}
        extensions: mbstring, intl
        ini-values: post_max_size=256M, max_execution_time=180
        coverage: xdebug        
        tools: php-cs-fixer, phpunit:${{ matrix.phpunit-versions }}
    - name: Validate composer.json and composer.lock
      run: composer validate --strict
    - name: Cache Composer packages
      id: composer-cache
      uses: actions/cache@v2
      with:
        path: vendor
        key: ${{ runner.os }}-php-${{ hashFiles('**/composer.lock') }}
        restore-keys: |
          ${{ runner.os }}-php-
    - name: Install dependencies
      run: composer install --prefer-dist --no-progress --no-suggest
    - name: Coverage
      run: vendor/bin/phpunit --coverage-clover coverage.xml
    - name: Codecov
      uses: codecov/codecov-action@v1  
```
