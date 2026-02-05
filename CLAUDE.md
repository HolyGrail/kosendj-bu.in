# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the website for 高専DJ部 (Kosen DJ Club), a static site built with Middleman 4. The site is event-driven, featuring DJ event information, timetables, member profiles, and venue details. It's automatically deployed to GitHub Pages when changes are pushed to master.

## Common Commands

### Development
```bash
# Start development server with live reload
bundle exec middleman server

# Watch and compile SCSS in parallel (automatically handled by middleman server)
pnpm run watch
```

Access the development site at `http://localhost:4567` (default Middleman port).

### Building
```bash
# Build static site
bundle exec middleman build

# Build CSS only
pnpm run build
```

The build output goes to the [build/](build/) directory.

### Setup (first time)
```bash
bundle install  # Install Ruby dependencies
pnpm install    # Install Node.js dependencies
```

## Architecture

### Template System
- **Templating**: Uses [Slim](http://slim-lang.com/) templates (Ruby-based)
- **Main page**: [source/index.html.slim](source/index.html.slim) - Single-page site with sections for event, timetable, members, and access
- **Layout**: [source/layouts/layout.slim](source/layouts/layout.slim) - Base HTML structure with meta tags, Google Analytics

### Styling Pipeline
- **SCSS files**: [source/stylesheets/](source/stylesheets/)
  - [all.css.scss](source/stylesheets/all.css.scss) - Main stylesheet
  - [_definition.scss](source/stylesheets/_definition.scss) - Variables and mixins
  - [_hero.scss](source/stylesheets/_hero.scss) - Hero section styles
- **External pipeline**: Middleman uses pnpm to run Sass independently ([config.rb:3-7](config.rb#L3-L7))
- **Output**: Compiled CSS goes to `.tmp/all.css` and is served by Middleman

### Data-Driven Content
The site is driven by YAML data files in [data/](data/):

- **[data/event.yml](data/event.yml)**: Event date and DJ timetable
  - `date`: Event date/time string
  - `timetable`: Array of DJ sets with `time`, `screen_name`, and `genre`

- **[data/members.yml](data/members.yml)**: Member profiles
  - Each member has: `name`, `icon`, `description`, `activities`, `services` (social links)

To update event information or add members, edit these YAML files directly. The templates automatically iterate over the data.

### Deployment
- **Automatic**: GitHub Actions deploys to GitHub Pages on push to master ([.github/workflows/deploy.yml](.github/workflows/deploy.yml))
- **Process**: Runs `bundle exec middleman build` and publishes the `build/` directory
- **Build checks**: PR builds are validated via [.github/workflows/build.yml](.github/workflows/build.yml)

### Configuration
- **[config.rb](config.rb)**: Middleman configuration
  - Activates livereload for development
  - Sets up external Sass pipeline
  - Ignores source SCSS files (handled by external pipeline)

## Project Structure
```
source/
├── index.html.slim       # Main single-page site
├── layouts/
│   └── layout.slim       # Base HTML layout
├── stylesheets/          # SCSS files (compiled externally)
└── images/               # Static images

data/
├── event.yml             # Event and timetable data
└── members.yml           # Member profiles

build/                    # Generated static site (git-ignored)
.tmp/                     # Compiled CSS from Sass
```

## Technology Stack
- **Ruby 3.3** (specified in [.ruby-version](.ruby-version))
- **Node.js** (version in [.node-version](.node-version))
- **pnpm 10.11.1** (package manager, specified in [package.json:9](package.json#L9))
- **Middleman 4** - Static site generator
- **Slim** - Template language
- **Sass** - CSS preprocessor
