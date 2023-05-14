```json
{
    "name": "asakurayoh/library-management",
    "description": "Library management system",
    "license": "GPL-2.0-or-later",
    "type": "typo3-cms-extension",
    "authors": [
        {
            "name": "Maxime Lafontaine",
            "email": "maxime.lafontaine@gmail.com",
            "role": "developer"
        }
    ],
    "homepage": "https://github.com/asakurayoh/library-management/",
    "support": {
        "issues": "https://github.com/asakurayoh/library-management/issues",
        "source": "https://github.com/asakurayoh/library-management"
    },
    "require": {
        "php": "~8.1.0 || ~8.2.0",
        "typo3/cms-core": "^12.4",
        "typo3/cms-extbase": "^12.4"
    },
    "require-dev": {
        "ergebnis/composer-normalize": "^2.31",
        "friendsofphp/php-cs-fixer": "^3.16",
        "helmich/typo3-typoscript-lint": "^3.1",
        "phpstan/extension-installer": "^1.3",
        "phpstan/phpstan": "^1.10",
        "phpstan/phpstan-phpunit": "^1.3",
        "phpstan/phpstan-strict-rules": "^1.5",
        "phpunit/phpunit": "^10.1",
        "saschaegerer/phpstan-typo3": "^1.8",
        "seld/jsonlint": "^1.9",
        "squizlabs/php_codesniffer": "^3.7",
        "typo3/coding-standards": "^0.7.1",
        "typo3/testing-framework": "^8.0"
    },
    "autoload": {
        "psr-4": {
            "AsakuraYoh\\LibraryManagement\\": "Classes/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "AsakuraYoh\\LibraryManagement\\Tests\\": "Tests/"
        }
    },
    "config": {
        "allow-plugins": {
            "ergebnis/composer-normalize": true,
            "typo3/class-alias-loader": true,
            "typo3/cms-composer-installers": true,
            "phpstan/extension-installer": true
        },
        "bin-dir": ".Build/bin",
        "preferred-install": {
            "*": "dist"
        },
        "sort-packages": true,
        "vendor-dir": ".Build/vendor"
    },
    "extra": {
        "typo3/cms": {
            "app-dir": ".Build",
            "extension-key": "library_management",
            "web-dir": ".Build/Web"
        }
    },
    "scripts": {
        "post-autoload-dump": [
            "@link-extension"
        ],
        "ci": [
            "@ci:static",
            "@ci:dynamic"
        ],
        "ci:composer:normalize": "@composer normalize --no-check-lock --dry-run",
        "ci:composer:psr-verify": "@composer dumpautoload --optimize --strict-psr",
        "ci:dynamic": [
            "@ci:tests"
        ],
        "ci:json:lint": "find . ! -path '*.Build/*' ! -path '*node_modules/*' -name '*.json' | xargs -r php .Build/bin/jsonlint -q",
        "ci:php": [
            "@ci:php:cs-fixer",
            "@ci:php:lint",
            "@ci:php:sniff",
            "@ci:php:stan"
        ],
        "ci:php:cs-fixer": "php-cs-fixer fix --config .php-cs-fixer.php -v --dry-run --using-cache no --diff",
        "ci:php:lint": "find .*.php *.php Classes Configuration Tests -name '*.php' -print0 | xargs -r -0 -n 1 -P 4 php -l",
        "ci:php:sniff": "phpcs Classes Configuration Tests",
        "ci:php:stan": "phpstan --no-progress",
        "ci:static": [
            "@ci:composer:normalize",
            "@ci:json:lint",
            "@ci:php:cs-fixer",
            "@ci:php:lint",
            "@ci:php:sniff",
            "@ci:php:stan",
            "@ci:ts:lint",
            "@ci:yaml:lint"
        ],
        "ci:tests": [
            "@ci:tests:unit",
            "@ci:tests:functional"
        ],
        "ci:tests:functional": "find 'Tests/Functional' -wholename '*Test.php' | parallel --gnu 'echo; echo \"Running functional test suite {}\"; .Build/bin/phpunit --testdox -c .Build/vendor/typo3/testing-framework/Resources/Core/Build/FunctionalTests.xml {}';",
        "ci:tests:unit": ".Build/bin/phpunit -c .Build/vendor/typo3/testing-framework/Resources/Core/Build/UnitTests.xml Tests/Unit --testdox",
        "ci:ts:lint": "typoscript-lint -c Configuration/TsLint.yml --ansi -n --fail-on-warnings -vvv Configuration/TypoScript",
        "ci:yaml:lint": "find . ! -path '*.Build/*' ! -path '*node_modules/*' -regextype egrep -regex '.*.ya?ml$' | xargs -r php ./.Build/bin/yaml-lint",
        "docs:generate": [
            "docker run --rm t3docs/render-documentation show-shell-commands > tempfile.sh; echo 'dockrun_t3rd makehtml' >> tempfile.sh; bash tempfile.sh; rm tempfile.sh"
        ],
        "fix:composer:normalize": "@composer normalize --no-check-lock",
        "fix:php": [
            "@fix:php:cs",
            "@fix:php:sniff"
        ],
        "fix:php:cs": "php-cs-fixer fix --config .php-cs-fixer.php",
        "fix:php:sniff": "phpcbf Classes Configuration Tests",
        "link-extension": [
            "@php -r 'is_dir($extFolder=__DIR__.\"/.Build/Web/typo3conf/ext/\") || mkdir($extFolder, 0777, true);'",
            "@php -r 'file_exists($extFolder=__DIR__.\"/.Build/Web/typo3conf/ext/library_management\") || symlink(__DIR__,$extFolder);'"
        ],
        "phpstan:baseline": ".Build/bin/phpstan --allow-empty-baseline --generate-baseline=phpstan-baseline.neon"
    },
    "scripts-descriptions": {
        "ci": "Runs all dynamic and static code checks.",
        "ci:composer:normalize": "Checks the composer.json.",
        "ci:composer:psr-verify": "Verifies PSR-4 namespace correctness.",
        "ci:dynamic": "Runs all PHPUnit tests (unit and functional).",
        "ci:json:lint": "Lints the JSON files.",
        "ci:php": "Runs all static checks for the PHP files.",
        "ci:php:cs-fixer": "Checks the code style with the PHP Coding Standards Fixer (PHP-CS-Fixer).",
        "ci:php:lint": "Lints the PHP files for syntax errors.",
        "ci:php:sniff": "Checks the code style with PHP_CodeSniffer (PHPCS).",
        "ci:php:stan": "Checks the PHP types using PHPStan.",
        "ci:static": "Runs all static code checks (syntax, style, types).",
        "ci:tests": "Runs all PHPUnit tests (unit and functional).",
        "ci:tests:functional": "Runs the functional tests.",
        "ci:tests:unit": "Runs the unit tests.",
        "ci:ts:lint": "Lints the TypoScript files.",
        "ci:yaml:lint": "Lints the YAML files.",
        "docs:generate": "Renders the extension ReST documentation.",
        "fix:composer:normalize": "Normalizes composer.json file content.",
        "fix:php": "Runs all fixers for the PHP code.",
        "fix:php:cs": "Fixes the code style with PHP-CS-Fixer.",
        "fix:php:sniff": "Fixes the code style with PHP_CodeSniffer.",
        "phpstan:baseline": "Updates the PHPStan baseline file to match the code.",
        "prepare-release": "Removes development-only files in preparation of a TER release."
    }
}
```
