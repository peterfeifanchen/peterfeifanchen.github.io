---
name: jekyll-server
description: >-
  Guides the agent on how to configure dependencies, resolve common macOS
  compatibility issues, and launch/manage the local Jekyll development server.
---

# Jekyll Local Development Server

## Overview
This skill provides instructions on how to install dependencies, resolve common macOS system Ruby/compilation errors (such as architecture conflicts with the `ffi` gem on Apple Silicon), and run the Jekyll development server locally for the PersonalWebsite repository.

## Dependencies
- Ruby (system Ruby `2.6.x` or higher)
- Bundler (`bundle`)
- Xcode Command Line Tools (for compiling native extensions)

## Quick Start
To immediately run the server (assuming dependencies are already installed):
```bash
bundle exec jekyll serve --host 127.0.0.1
```
The site will be available at [http://127.0.0.1:4000/](http://127.0.0.1:4000/).

## Workflow

### 1. Check Gemfile and Gemfile.lock
Ensure that:
- Gem names are specified using standard quotes (e.g., `gem "jekyll-seo-tag"`) rather than backticks.
- If running on Apple Silicon (arm64) macOS with system Ruby 2.6, the `ffi` gem must be pinned to a compatible version (such as `1.15.5`) in the `Gemfile` to prevent compilation errors or incompatible Ruby version requirements from newer precompiled binaries:
  ```ruby
  gem "ffi", "1.15.5"
  ```

### 2. Install/Update Dependencies Locally
Always install gems locally inside the project directory (`vendor/bundle`) to avoid permission issues with macOS system Ruby:
```bash
bundle install --path vendor/bundle
```
If there are version mismatches or lockfile conflicts, run:
```bash
bundle update ffi
```

### 3. Launch the Server
Launch the Jekyll server in the background:
```bash
bundle exec jekyll serve --host 127.0.0.1
```

### 4. Monitor and Verify
Check the server's output/logs to ensure it compiles the site and successfully listens on:
- Address: `http://127.0.0.1:4000/`

## Common Mistakes
- **Running `bundle install` globally**: Running without `--path vendor/bundle` can lead to permission issues on macOS system directories (e.g., `/Library/Ruby/Gems/2.6.0`).
- **Compiling old `ffi` versions on Apple Silicon**: Older versions like `ffi 1.9.x` will fail to compile on arm64 architectures. Pin to `1.15.5`.
- **Incompatible `ffi` versions**: Trying to use newer versions like `ffi 1.17.x` will fail on Ruby 2.6 because they require Ruby >= 3.0.
