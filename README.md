# apt-cache

Cache Apt Packages for GitHub Actions

## Description

This action allows you to install and cache apt packages to speed up your GitHub Actions workflows. It uses GitHub's cache action under the hood to store and retrieve `.deb` files, reducing the need to download packages repeatedly.

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `packages` | List of packages to install and cache | Yes | |
| `path` | Location to cache deb files | No | `.aptcache` |
| `run-update` | Whether to run apt-get update | No | `false` |

## Usage

### Basic Usage

```yaml
- name: Install packages with caching
  uses: rishabhc32/apt-cache
  with:
    packages: ansible fail2ban
```

### With apt-get update

```yaml
- name: Install packages with update
  uses: rishabhc32/apt-cache
  with:
    packages: ansible
    run-update: true
```

### Custom cache path

```yaml
- name: Install packages with custom cache path
  uses: rishabhc32/apt-cache
  with:
    packages: ansible fail2ban
    path: .custom-apt-cache
```

## How it works

1. Optionally runs `apt-get update` if `run-update` is set to `true`
2. Determines package versions using `apt-cache policy`
3. Creates a hash based on package names and versions
4. Checks if a cache exists for this hash
5. If cache exists, copies `.deb` files to the apt archives directory (`/var/cache/apt/archives`)
6. Installs the requested packages
7. If no cache was found, saves the downloaded `.deb` files from `/var/cache/apt/archives` to the cache path for future runs

> **Note:** The action interacts with the system's apt cache at `/var/cache/apt/archives`. It copies cached `.deb` files to this location when restoring from cache and copies newly downloaded packages from this location when saving to cache.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
