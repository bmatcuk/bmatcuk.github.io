bmatcuk.github.io
=================

Personal projects and musings of a lunatic.

# Building
Pretty straightforward:

```bash
bundle install

# build sass, images (only builds what needs updating)
rake build

# build jekyll
jekyll build
```

## Building on OSX
You need to install v6 of imagemagick and pkg-config:

```bash
brew install imagemagick@6 pkg-config
brew link --force imagemagick@6
bundle install
```
