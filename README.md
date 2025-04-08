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

1. Optionally runs `apt-get update` if enabled
2. Determines package versions using `apt-cache policy`
3. Creates a hash based on package names and versions
4. Checks for existing cache using this hash

> [!NOTE]
> The action copies cached `.deb` files to `/var/cache/apt/archives` when restoring from cache and copies newly downloaded packages from this location when saving to cache.

## Acknowledgments

This action was inspired by [Eeems-Org/apt-cache-action](https://github.com/Eeems-Org/apt-cache-action) with several improvements:

- Uses SHA256 for cache hashing
- Adds an optional `run-update` boolean parameter to skip running `apt update` when not needed

## License

This project is licensed under the MIT License - see the LICENSE file for details.
