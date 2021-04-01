<p align="center"><img src="https://raw.githubusercontent.com/svengreb/tmpl/main/assets/images/repository-hero.svg?sanitize=true"/></p>

<p align="center"><a href="https://github.com/svengreb/tmpl/releases/latest"><img src="https://img.shields.io/github/release/svengreb/tmpl.svg?style=flat-square&label=Release&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0"/></a> <a href="https://github.com/svengreb/tmpl/blob/master/CHANGELOG.md"><img src="https://img.shields.io/github/release/svengreb/tmpl.svg?style=flat-square&label=Changelog&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0"/></a></p>

<p align="center"><a href="https://github.com/svengreb/tmpl/actions?query=workflow%3Aci" target="_blank"><img src="https://img.shields.io/github/workflow/status/svengreb/tmpl/ci.svg?style=flat-square&label=CI&logo=github&logoColor=eceff4&colorA=4c566a"/></a></p>

<p align="center"><a href="https://github.com/arcticicestudio/styleguide-markdown/releases/latest"><img src="https://img.shields.io/github/release/arcticicestudio/styleguide-markdown.svg?style=flat-square&label=Markdown%20Style%20Guide&logoColor=eceff4&colorA=4c566a&colorB=88c0d0&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzOSIgaGVpZ2h0PSIzOSIgdmlld0JveD0iMCAwIDM5IDM5Ij48cGF0aCBmaWxsPSJub25lIiBzdHJva2U9IiNEOERFRTkiIHN0cm9rZS13aWR0aD0iMyIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBkPSJNMS41IDEuNWgzNnYzNmgtMzZ6Ii8%2BPHBhdGggZmlsbD0iI0Q4REVFOSIgZD0iTTIwLjY4MyAyNS42NTVsNS44NzItMTMuNDhoLjU2Nmw1Ljg3MyAxMy40OGgtMS45OTZsLTQuMTU5LTEwLjA1Ni00LjE2MSAxMC4wNTZoLTEuOTk1em0tMi42OTYgMGwtMTMuNDgtNS44NzJ2LS41NjZsMTMuNDgtNS44NzJ2MS45OTVMNy45MzEgMTkuNWwxMC4wNTYgNC4xNnoiLz48L3N2Zz4%3D"/></a> <a href="https://github.com/arcticicestudio/styleguide-git/releases/latest"><img src="https://img.shields.io/github/release/arcticicestudio/styleguide-git.svg?style=flat-square&label=Git%20Style%20Guide&logoColor=eceff4&colorA=4c566a&colorB=88c0d0&logo=git"/></a></p>

<p align="center">A collection of <a href="https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-template-repository" target="_blank">template repositories</a> for new projects.</p>

Starting a new project repository means to spend a lot of time first for setting up the basic structure and configuration for the used technology stack over and over again. It is a repetitive task since the same tools are often used across many projects, like linters and code formatters, and therefore often just copied over. This has the disadvantage that the configuration or documentation may differ between different projects or common changes and configurations are not synchronize at all. Many tools provide a way to created so called “shared configurations“ that can be used as base and dynamically composed with project-specific configurations, however, this does not apply to all.
To prevent such problems a central place, so called “template repository“, for basic configurations and documentations as well as focus on different use cases, project structures and programming languages is a good solution to act as a single-source-of-truth™️.

_tmpl_, the imaginative abbreviation for “template“, is my solution to enable a smooth project start and ensure that a project stays healthy, stable and up-to-date over time. It is separated into context, layout and/or language specific templates where each lives in its own repository.
Please note that _tmpl_ has mainly been created for my personal use in mind. The default configurations of these templates might not fit your needs, but they were designed to be flexible and can be adapted to different use cases and environments.

## Overview

This repository serves as the base template repository of _tmpl_ that provides essential features for every project.

- [GitHub specific features](#github) like…
  - …definitions for [code owners](#code-owners)
  - …individual [issue templates](#issue-templates)
  - …a extensive [pull request template](#pull-request-template)
  - …a basic [CI/CD action workflow](#cicd-action-workflow)
  - …[automated updates and security vulnerability alerts](#automated-dependency-updates) for dependencies through the native [Dependabot][] integration
- Configurations for [npm and Yarn](#nodejs-npm-and-yarn)
- Basic configurations for [Git](#git)
- A [MIT license](#license)
- [EditorConfig](#editorconfig) integration
- [Prettier][] for [“auto-magical“ code& text formatting](#automatic-code-and-text-formatter)
- [remark-lint][] to [lint Markdown](#markdown-linting)
- [lint-staged][] for [Automatic pre-commit linting](#automatic-pre-commit-linting)
- [husky][] to [manage and run Git Hooks](#git-hook-manager-&-runner)

See the [“Features“](#features) section below for more details.

## Template Repositories

Like mentioned in the introduction above, _tmpl_ is separated into multiple scalable _tmpl_ repositories based on different project layouts, languages and/or context. See the documentations of the specific repository for more details about the use case(s) and how to use it as template:

- [tmpl-go][] — A template repository for [Go][] projects.

## Directory Structure

```raw
tmpl
├─ .github
│   ├─ ISSUE_TEMPLATE
│   └─ workflows
└─ assets
    └─ images
```

The current directory structure consists of basic directories mainly targeted for repositories hosted on GitHub, but the contained files can also be simply adjusted for usage with other platforms. Please see the [GitHub Features](#github-features) section below for more details about the provided files.

- `.github` — The base directory for configurations and documentations of [GitHub features][gh-features] like [action][gh-docs-act] workflows and [issue/pull request templates][gh-docs-comm-issue_pr_tmpl].
  - `.github/ISSUE_TEMPLATE` — The directory for [individual issue template files][gh-docs-comm-issue_tmpl].
  - `.github/workflows` — The directory for [action workflow files][gh-docs-act-ref-syntax].
- `.assets` — A common base directory for (binary) projects assets like media or tool specific files that is independent of the repository hosting platform.
  - `.assets/images` — The sub-directory for project and/or repository specific images like [SVG][wikip-svg] or [raster graphics][wikip-raster_graphics].

## Features

This repository serves as the base template repository of _tmpl_ that provides essential features for every project.

The sections below contain more detailed information about each feature, the sane default configurations and their files in the repository as well as ways to customize and adjust them to adapt to other use cases and projects. They provide details about files in this repository and the overall directory structure.

### GitHub

This template repository has partially been designed for repositories hosted on GitHub and makes use of many of [its fantastic features][gh-features].

#### Code Owners

The [`.github/CODEOWNERS`][gh-docs-code_owners] file defines individuals or teams that are responsible for code in a repository and automatically requested for review when a contributor opens a pull request that modifies files that they own.

#### Issue Templates

The [individual issue template files][gh-docs-comm-issue_tmpl] in the `.github/ISSUE_TEMPLATE` directory are automatically used as content when a contributor creates a new issue where each file will be available as template in the [issue template chooser][gh-docs-comm-issue_tmpl-chooser].
Note that the [`.github/ISSUE_TEMPLATE.md` file][gh-docs-issue_tmpl_legacy] is still included in this repository, but has been official deprecated by GitHub in favor of these individual issue template files.

#### Pull Request Template

The [`.github/PULL_REQUEST_TEMPLATE.md`][gh-docs-comm-pr_tmpl] file is automatically used as content when a contributor creates a new pull request. It can also be used for automation and [as URL query parameter value][gh-docs-autom_issue_pr_query] e.g. do directly link to it in documentations.

#### CI/CD Action Workflow

The [GitHub Actions][gh-feat-actions] `.github/workflows` directory includes a basic [CI/CD workflow file][gh-docs-act-ref-syntax] that runs for changes in the Git `main` branch and `v*` tags. There is currently one job called `lint-node` that runs all linters that are included in this template repository. See sections like [“Automatic Code and Text Formatter“](#automatic-code-and-text-formatter), [“Markdown Linting“](#markdown-linting) and [“Automatic Pre-Commit Linting“](#automatic-pre-commit-linting) below for more details.

#### Automated Dependency Updates

To ensure dependencies of various package ecosystems are up-to-date, [Dependabot][] has been [natively integrated into GitHub][gh-blog-dependabot]. It creates [automated updates][gh-docs-dependabot] and [security vulnerability alerts][gh-docs-sec_vuls-alerts] for dependencies.
The basic [`dependabot.yml` file][gh-blob-dot_github-dependabot.yml] in this template repository contains [the configuration][gh-docs-dependabot] for [GitHub Action workflows](#cicd-action-workflow) and [Node.js dependencies](#nodejs-npm-and-yarn).

Note that you must manually enable or disable [version][gh-docs-dependabot_activation] and [security][gh-docs-sec_vuls-dependabot_config] Dependabot updates per repository!

### Node.js: npm and Yarn

Since most of the tools included in this template repository like…

- Prettier for [“auto-magical“ code & text formatting](#automatic-code-and-text-formatter)
- remark-lint for [linting of Markdown source files](#markdown-linting)
- lint-staged for [automatic pre-commit linting](#automatic-pre-commit-linting)
- husky as [Git Hook manager & runner](#git-hook-manager-&-runner)

…are (unfortunately) written in JavaScript for [Node.js][], [npm][]/[Yarn][] is required to manage these packages.

The [`package.json`][npm-docs-pkg.json] and [`.npmrc`][npm-docs-v7-npmrc] files declare [Node.js][] specific configurations like the development dependencies and the [`package-lock`][npm-docs-config#package-lock], [`save-exact`][npm-docs-config#save-exact] and [`save-prefix`][npm-docs-config#save-prefix] version resolution strategy. npm is the main package manager so Yarn‘s [`yarn.lock`][yarn-docs-yarn.lock] file should not be used and tracked in order to prevent conflicts with the [`package-lock.json`][npm-docs-pkg-lock.json] file.

Note that as of [version `0.9.0`][gh-ms-9] the minimum versions for npm is [`>=7.7.0`][gh-npm/cli-rel-v7.7.0], bundled with [Node `>=15.13.0`][node-dist-v15.13.0], in order to use the `run` and `exec` commands with [workspaces][npm-docs-ws].

### Git

The [`.gitattributes`][git-docs-attr] and [`.gitignore`][git-docs-ignore] configuration files define basic pattern. This includes pattern when using [git-crypt][] to encrypt and store secrets within the repository as well as pattern for the fantastic [JetBrains][] products like [IntelliJ][].

There is also the [`.mailmap`][git-docs-mailmap] file that…

1. …serves as a reference to involved project maintainers and teams and also allows contributors to send mails regarding security issues.
2. …prevents unnecessary overhead of updating all documentations when new core team members and contributors are added.
3. …fulfills it main purpose and functionality: Mapping commit authors when someone changed their email address or uses a different one.

### License

The _tmpl_ project and its repositories are licensed under the [MIT license][license-mit] which is included as [`LICENSE` file][gh-blob-license].

### EditorConfig

The [`.editorconfig`][editorconfig] file allows to define and maintain consistent coding styles between different editors and IDEs. It is a well-known file that can, next to tools that are specialized to specific languages and formats, help to keep the style of source code and documentations healthy.

### Automatic Code and Text Formatter

A code formatter is a essential part of a project setup to ensure a good and consistent code style without requiring relatively time-consuming manual corrections found by a code linter. With code being automatically formatted on actions like saving a file the developer can focus entirely on the code instead of spending time and energy on indenting code line by line.

That‘s where one special project comes in: [Prettier][], the opinionated code formatter with support for almost any language and integration with almost every popular editor. I‘ve been using it since the first version and I totally forgot about the fact that formatting is even a thing. That could also be because [Gophers][go-blog-gopher] are already used to [`gofmt`][golang-docs-gofmt] anyway.

_Prettier_ is a absolute must-have for every project setup and I‘m not aware of any other projects with such advanced parsers and language support. The only negative point is that it is written in _JavaScript_ instead of [Go][] so it always pulls in [Node.js][] as a development dependency. This is not a problem at all for web-based projects, but for Go or any other non Node.js project it inflates the setup unnecessarily.

Anyway, the fantastic developer experience and project benefits clearly outweigh the negative points. In addition many developers today already have _Node_ installed locally since its large ecosystem has already spread by far further than just the web but already powers many system, desktop and CLI applications.

One of the main features of Prettier is its configuration: It already provides the best and recommended style configurations of-out-the-box™.
The only option that was adjusted for this template repository is the [print width][prettier-docs-opt#pwidth]. It is set to `80` by default which is not up-to-date for modern screens (might only be relevant when working in [TUI][wikip-tui]s like e.g. [Vim][]). It has been changed to `120` like defined in [all of my style guides][gh-arcticicestudio-repos-q-stg].
The `prettier.config.js` configuration file is placed in the project root as well as the `.prettierignore` file that defines ignore pattern.

To allow to format all sources a `format:pretty` npm package script is available that also runs in the main `format` script flow. The `lint:pretty` script allows to check if all supported files are formatted correctly and is also included in the main `lint` script flow.

### Markdown Linting

Ensuring that documentations and content written in [wikip-markdown][] are of great quality should be a continuous goal of any project. Persisting information is a consistent and extensive way helps tp keep a project healthy, no matter whether for long-time or new users ad well as maintainers and contributors. Linting Markdown results in better rendering with different markdown parsers and makes sure less refactoring is needed afterwards.

The best solution for this task would be a tool written in [Go][], but the undisputed best tool is still [remark-lint][] which is (unfortunately) written in JavaScript and used via [Node.js][]. Of course there are fantastic projects written in Go like [goldmark][] that provides a great way to _parse_ content, but linting is (currently) not a feature.
remark-lint on the other side is built on top of [remark][], the powerful Markdown processor powered by plugins (such as remark-lint itself) and part of the [unifiedjs][] collective. It comes with a really [large set of customizable rule][remark-docs-rules] and ways to ensure Markdown will be consistent across any project.

Another decision point for remark-lint was the fact that Prettier (see section [“Automatic Code and Text Formatter“](#automatic-code-and-text-formatter)) so Node.js is already a development dependency anyway. This also allows to add other awesome projects that are (unfortunately) written in JavaScript and for which there is no comparable alternative.

remark-lint can be used via [remark-cli][npm-remark-cli] and a rule preset. This preset is [remark-preset-lint-arcticicestudio][gh-arcticicestudio/remark-preset-lint-arcticicestudio], my custom preset that implements my [Markdown Style Guide][arcticicestudio-stg-markdown].
Since the custom preset is still in major version `0` the version range is currently `>=0.x.x <1.0.0` to avoid the “SemVer Major Zero Caveat”. When defining package versions with the the carat `^` or tilde `~` range selector it won‘t affect packages with a major version of `0`. npm will resolve these packages to their exact version until the major version is greater or equal to `1`.
To avoid this caveat the more detailed version range `>=0.x.x <1.0.0` is used to resolve all versions greater or equal to `0.x.x` but less than `1.0.0`. This will always use the latest `0.x.x` version and removes the need to increment the version manually on each new release.

The `.remarkrc.js` configuration file is placed in the project root as well as the `.remarkignore` file that defines ignore pattern.

To allow to run the Markdown linting separately the `lint:md` npm package script is available and included in the main `lint` script flow.

### Automatic Pre-Commit Linting

[Git Hooks][git-book-hooks] are a fantastic way to customize the development workflow of a project to simplify and automate specific tasks that are required when working on the code base. For example, formatting, linting and running tests before pushing a commit to ensure it conforms to the code style and works as expected.
Most Git Hooks are not that complex and fullfil a simple purpose while other solutions like [Danger][] can help to manage larger projects and projects that need to scale.

This template repository uses a Git Hook that automatically runs configured linters on all files that have been staged and that match the configured pattern like file extensions or names.
Like described in the [“Automatic Code and Text Formatter“](#automatic-code-and-text-formatter) section above, [Node.js][] is already a development dependency anyway so the [lint-staged][] NPM package is used for this goal. I‘ve used this package in almost any project and it is the most stable, production-proven and advanced tool that is currently out there with no comparable alternatives in other languages.

The configuration file `lint-staged.config.js` is placed in the project root and includes the command that should be run for matching file extensions. It currently contains entries for the following linters that run in the same order as listed here:

1. `prettier --list-different` - Runs [Prettier][] (see section [“Automatic Code and Text Formatter“](#automatic-code-and-text-formatter)) to ensure all files are formatted correctly. The `--check` flag prints files that are not conform with the Prettier configuration.
2. `remark --no-stdout` - Runs [remark-lint][] (see section [“Markdown Linting“](#markdown-linting)) against `*.md` to ensure all Markdown files are compliant to the style guide. The `--no-stdout` flag suppresses the output of the parsed file content.

### Git Hook Manager & Runner

In section [“Automatic Pre-Commit Linting“](#automatic-pre-commit-linting) the integration of [lint-staged][], a [Git Hook][git-book-hooks] runner that runs linters against staged files, is described and how it is used in this template repository. To automatically run it and other Git Hooks the hook manager and runner [husky][] is used.

Just like other already added tools described in the sections above, husky is (unfortunately) also written in JavaScript. Since [Node.js][] is therefore already a development dependency it doesn‘t really matter that husky is another Node.js package too.
Unlike the other tools there are indeed alternatives written in [Go][] like [lefthook][] or [quickhook][], but it requires time to test and evaluate them before actually replacing husky. Also a long as there are no comparable alternatives to the already used tools listed above, this template would be more complex by requiring both Node.js and Go as development dependency. Therefore husky took over the part as hook manager & runner for this template repository since it is a stable, production-proven and advanced project that I already use in almost any other project setup.

The `.huskyrc.js` configuration file is placed in the project root and includes the command to run for any [supported Git Hook][husky-docs-hooks]. It contains entries for the following hooks:

- `pre-commit` - Runs lint-staged (see section [“Automatic Pre-Commit Linting“](#automatic-pre-commit-linting)) before each commit to ensure all staged files are compliant to all style guides.

## Usage

There are multiple ways to use this template repository, either by [using GitHubs feature to create a repository from a template][gh-docs-repo_from_tmpl] or simply [cloning it][gh-docs-repo_clone]. No matter which method is used, there are a few manual steps to adjust some configurations and documentations for the individual target project. Note that these changes are only recommendations, but are common steps anyway.

1. Adjust the [`.github/CODEOWNERS`][gh-blob-codeowners] and [`.mailmap`][gh-blob-mailmap] files to use mappings for your project contributors and team members.
2. Adjust [files related to GitHub features](#github) like the [CI/CD action workflow](#cicd-action-workflow), [issue](#issue-templates) & [pull request](#pull-request-template) templates, [code owners](#code-owners) and any other file located in the [`github` directory][gh-tree-dot_github] to match your project.
3. Adjust documentations like the [README][gh-blob-readme], [changelog][gh-blob-changelog], [code of conduct][gh-blob-coc] and [contribution guides][gh-blob-contrib] to match your project.
4. Adjust the [`package.json`][gh-blob-pkg.json] file to match your project.
5. Adjust the [`dependabot.yml`][gh-blob-dot_github-dependabot.yml] file to match your project.
6. Adjust all copyright notices to match your project.

## Contributing

_tmpl_ is an open source project and contributions are always welcome!

There are many ways to contribute, from [writing- and improving documentation and tutorials][contrib-guide-docs], [reporting bugs][contrib-guide-bugs], [submitting enhancement suggestions][contrib-guide-enhance] that can be added to _tmpl_ by [submitting pull requests][contrib-guide-pr].

Please take a moment to read the [contributing guide][contrib-guide] to learn about the development process, the [styleguides][contrib-guide-styles] to which this project adheres as well as the [branch organization][contrib-guide-branching] and [versioning][contrib-guide-versioning] model.

The guide also includes information about [minimal, complete, and verifiable examples][contrib-guide-mcve] and other ways to contribute to the project like [improving existing issues][contrib-guide-impr-issues] and [giving feedback on issues and pull requests][contrib-guide-feedback].

<p align="center">Copyright &copy; 2020-present <a href="https://www.svengreb.de" target="_blank">Sven Greb</a></p>

<p align="center"><a href="https://github.com/svengreb/tmpl/blob/main/LICENSE"><img src="https://img.shields.io/static/v1.svg?style=flat-square&label=License&message=MIT&logoColor=eceff4&logo=github&colorA=4c566a&colorB=88c0d0"/></a></p>

[arcticicestudio-stg-markdown]: https://arcticicestudio.github.io/styleguide-markdown
[contrib-guide-branching]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#branch-organization
[contrib-guide-bugs]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#bug-reports
[contrib-guide-docs]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#documentations
[contrib-guide-enhance]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#enhancement-suggestions
[contrib-guide-feedback]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#give-feedback-on-issues-and-pull-requests
[contrib-guide-impr-issues]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#improve-issues
[contrib-guide-mcve]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#mcve
[contrib-guide-pr]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#pull-requests
[contrib-guide-styles]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#style-guides
[contrib-guide-versioning]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md#versioning
[contrib-guide]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md
[danger]: https://danger.systems
[dependabot]: https://dependabot.com
[editorconfig]: https://editorconfig.org
[gh-arcticicestudio-repos-q-stg]: https://github.com/arcticicestudio?tab=repositories&q=styleguide
[gh-arcticicestudio/remark-preset-lint-arcticicestudio]: https://github.com/arcticicestudio/remark-preset-lint-arcticicestudio
[gh-blob-changelog]: https://github.com/svengreb/tmpl/blob/main/CHANGELOG.md
[gh-blob-coc]: https://github.com/svengreb/tmpl/blob/main/CODE_OF_CONDUCT.md
[gh-blob-codeowners]: https://github.com/svengreb/tmpl/blob/main/.github/CODEOWNERS
[gh-blob-contrib]: https://github.com/svengreb/tmpl/blob/main/CONTRIBUTING.md
[gh-blob-dot_github-dependabot.yml]: https://github.com/svengreb/tmpl-go/blob/main/.github/dependabot.yml
[gh-blob-license]: https://github.com/svengreb/tmpl/blob/main/LICENSE
[gh-blob-mailmap]: https://github.com/svengreb/tmpl/blob/main/.mailmap
[gh-blob-pkg.json]: https://github.com/svengreb/tmpl/blob/main/package.json
[gh-blob-readme]: https://github.com/svengreb/tmpl/blob/main/README.md
[gh-blog-dependabot]: https://github.blog/2020-06-01-keep-all-your-packages-up-to-date-with-dependabot
[gh-docs-act-ref-syntax]: https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions
[gh-docs-act]: https://docs.github.com/en/free-pro-team@latest/actions
[gh-docs-autom_issue_pr_query]: https://docs.github.com/en/free-pro-team@latest/github/managing-your-work-on-github/about-automation-for-issues-and-pull-requests-with-query-parameters
[gh-docs-code_owners]: https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/about-code-owners
[gh-docs-comm-issue_pr_tmpl]: https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/about-issue-and-pull-request-templates
[gh-docs-comm-issue_tmpl-chooser]: https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/configuring-issue-templates-for-your-repository#configuring-the-template-chooser
[gh-docs-comm-issue_tmpl]: https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/configuring-issue-templates-for-your-repository
[gh-docs-comm-pr_tmpl]: https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository
[gh-docs-dependabot_activation]: https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/enabling-and-disabling-version-updates
[gh-docs-dependabot]: https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/configuration-options-for-dependency-updates
[gh-docs-issue_tmpl_legacy]: https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/manually-creating-a-single-issue-template-for-your-repository
[gh-docs-repo_clone]: https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/cloning-a-repository
[gh-docs-repo_from_tmpl]: https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template
[gh-docs-sec_vuls-alerts]: https://docs.github.com/en/free-pro-team@latest/github/managing-security-vulnerabilities/about-alerts-for-vulnerable-dependencies
[gh-docs-sec_vuls-dependabot_config]: https://docs.github.com/en/free-pro-team@latest/github/managing-security-vulnerabilitiesconfiguring-dependabot-security-updates
[gh-feat-actions]: https://github.com/features/actions
[gh-features]: https://github.com/features
[gh-ms-9]: https://github.com/svengreb/tmpl/milestone/9
[gh-npm/cli-rel-v7.7.0]: https://github.com/npm/cli/releases/tag/v7.7.0
[gh-tree-dot_github]: https://github.com/svengreb/tmpl/tree/main/.github
[git-book-hooks]: https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks
[git-crypt]: https://github.com/AGWA/git-crypt
[git-docs-attr]: https://git-scm.com/docs/gitattributes
[git-docs-ignore]: https://git-scm.com/docs/gitignore
[git-docs-mailmap]: https://git-scm.com/docs/git-shortlog#_mapping_authors
[go-blog-gopher]: https://blog.golang.org/gopher
[go]: https://go.dev
[golang-docs-gofmt]: https://golang.org/cmd/gofmt
[goldmark]: https://github.com/yuin/goldmard
[husky-docs-hooks]: https://github.com/typicode/husky/blob/master/DOCS.md#supported-hooks
[husky]: https://github.com/typicode/husky
[intellij]: https://www.jetbrains.com/idea
[jetbrains]: https://www.jetbrains.com
[lefthook]: https://github.com/Arkweid/lefthook
[license-mit]: https://opensource.org/licenses/MIT
[lint-staged]: https://github.com/okonet/lint-staged
[node-dist-v15.13.0]: https://nodejs.org/dist/v15.13.0
[node.js]: https://nodejs.org
[npm-docs-config#package-lock]: https://docs.npmjs.com/cli/v6/using-npm/config#package-lock
[npm-docs-config#save-exact]: https://docs.npmjs.com/cli/v6/using-npm/config#save-exact
[npm-docs-config#save-prefix]: https://docs.npmjs.com/cli/v6/using-npm/config#save-prefix
[npm-docs-pkg-lock.json]: https://docs.npmjs.com/files/package-lock.json
[npm-docs-pkg.json]: https://docs.npmjs.com/files/package.json
[npm-docs-v7-npmrc]: https://docs.npmjs.com/cli/v7/configuring-npm/npmrc
[npm-docs-ws]: https://docs.npmjs.com/cli/v7/using-npm/workspaces
[npm-remark-cli]: https://www.npmjs.com/package/remark-cli
[npm]: https://www.npmjs.com
[prettier-docs-opt#pwidth]: https://prettier.io/docs/en/options.html#print-width
[prettier]: https://prettier.io
[quickhook]: https://github.com/dirk/quickhook
[remark-docs-rules]: https://github.com/remarkjs/remark-lint/blob/master/doc/rules.md
[remark-lint]: https://github.com/remarkjs/remark-lint
[remark]: https://remark.js.org
[tmpl-go]: https://github.com/svengreb/tmpl-go
[unifiedjs]: https://unifiedjs.com
[vim]: https://www.vim.org
[wikip-markdown]: https://en.wikipedia.org/wiki/Markdown
[wikip-raster_graphics]: https://en.wikipedia.org/wiki/Raster_graphics
[wikip-svg]: https://en.wikipedia.org/wiki/Scalable_Vector_Graphics
[wikip-tui]: https://en.wikipedia.org/wiki/Text-based_user_interface
[yarn-docs-yarn.lock]: https://yarnpkg.com/lang/en/docs/yarn-lock
[yarn]: https://yarnpkg.com
