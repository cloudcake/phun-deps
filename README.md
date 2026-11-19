# phun-deps

Builds of PHP and the services that [Phun](https://github.com/cloudcake/phun)
installs where upstream publishes no build for a platform. Each workflow builds
from the upstream source, checks that the result starts and answers, and
publishes the archives with a `SHA256SUMS` on a release tagged
`<name>-<version>`. Phun pins those checksums in `resources/downloads.json`
with `scripts/pin-php.sh <tag>` or `scripts/pin-services.sh <tag>`.

| Workflow | Release tag | Platforms | Why |
|---|---|---|---|
| `php-build` | `php-<version>` | Linux (x86_64, aarch64), macOS (Apple Silicon, Intel) | PHP 8.4, 8.5 and 8.6, cli + fpm, built with static-php-cli so they run without system libraries |
| `php-ext-windows` | `php-ext-windows-<version>` | Windows (x86_64) | The extensions `php-build` has that the official windows.php.net NTS zip lacks (apcu, redis, imagick, memcached, event, protobuf, opentelemetry) in one zip, plus pcov and Xdebug as separate on-demand DLLs, built with the PHP SDK for that zip; PECL publishes no PHP 8.6 Windows builds |
| `valkey-build` | `valkey-<version>` | macOS (Apple Silicon, Intel) | Valkey publishes Linux tarballs only |
| `mariadb-build` | `mariadb-<version>` | macOS (Apple Silicon, Intel) | MariaDB publishes Linux and Windows only |
| `memcached-build` | `memcached-<version>` | Linux (x86_64, aarch64), macOS (Apple Silicon, Intel) | memcached.org publishes source only |

All workflows are manual (`workflow_dispatch`): run them from the Actions tab.
Each one refuses to replace an existing release; delete it first to rebuild.
The archives keep each project's own licence: PHP is under the PHP License,
Valkey and memcached are BSD-3-Clause, MariaDB is GPL-2.0. The Windows
extension archives carry each extension's and library's licence under
`licenses/`.

On Windows the rest of `php-build`'s extension list is already in the
official zip (built in, or as `ext\php_*.dll`), except pcntl, posix, sysvmsg
and sysvsem, which PHP does not build on Windows. `php-ext-windows` checks
that every other one loads, together, with `php -m`.
