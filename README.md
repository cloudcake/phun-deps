# phun-deps

Builds of PHP and the services that [Phun](https://github.com/cloudcake/phun)
installs where upstream publishes no build for a platform. Each workflow builds
from the upstream source, checks that the result starts and answers, and
publishes the archives with a `SHA256SUMS` on a release tagged
`<name>-<version>`. Phun pins those checksums in `resources/downloads.json`
with `scripts/pin-php.sh <tag>` or `scripts/pin-services.sh <tag>`.

| Workflow | Release tag | Platforms | Why |
|---|---|---|---|
| `php-build` | `php-<version>` | Linux (x86_64, aarch64), macOS (Apple Silicon, Intel) | PHP 8.4 and 8.5, cli + fpm, built with static-php-cli so they run without system libraries |
| `valkey-build` | `valkey-<version>` | macOS (Apple Silicon, Intel) | Valkey publishes Linux tarballs only |
| `mariadb-build` | `mariadb-<version>` | macOS (Apple Silicon, Intel) | MariaDB publishes Linux and Windows only |
| `memcached-build` | `memcached-<version>` | Linux (x86_64, aarch64), macOS (Apple Silicon, Intel) | memcached.org publishes source only |

All workflows are manual (`workflow_dispatch`): run them from the Actions tab.
Each one refuses to replace an existing release; delete it first to rebuild.
The archives keep each project's own licence: PHP is under the PHP License,
Valkey and memcached are BSD-3-Clause, MariaDB is GPL-2.0.
