<p align="center"><img src="https://raw.githubusercontent.com/svengreb/tmpl/main/assets/images/repository-hero.svg?sanitize=true"/></p>

<p align="center">Changelog for the collection of <a href="https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-template-repository" target="_blank">template repositories</a> for new projects.</p>

<!--lint disable no-duplicate-headings no-duplicate-headings-in-section-->

# 0.9.0

![Release Date: 2021-04-01](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2021-04-01&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.9.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/12) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.9.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/9)

⇅ [Show all commits][gh-compare-tag-v0.8.0_v0.9.0]

## Improvements

<details>
<summary><strong>From npm to Yarn and back again</strong> — #72 ⇄ #73 (⊶ b9967864)</summary>

↠ Some years ago, the switch from [npm][] (`v4`) to [Yarn][yarn-v1] (`v1`) was mainly done because of the fantastic [workspace feature][yarn-docs-ws] for [monorepos][wiki-monorepo] as well as the great performance and UX improvements. This was a good decision and almost every popular and well-known project used to do the same, but with the announcement of [Yarn v2][yarn] (named [“berry“][gh-yarnpkg/berry]) the community got upset about the path the project has taken. Next to this, [npm joined GitHub][gh-blog-npm_joins] back in March 2020 which meant that the development continues in a good direction and is baked by the open source platform itself.
These events, the overall fantastic new npm release version `v7`, including the introduction of [workspaces][npm-docs-ws], and the fact that I never liked the disadvantage of requiring to use an “external“ package manager instead of the one that is bundled with Node, lead to the decision to finally switch back to npm again.

The only drawback is the constraint that the minimum [npm version is now `v7.7.0`][gh-npm/cli-rel-v7.7.0] because this is the first version that comes with workspace support for the `run-script` and `exec` commands through the `--workspace`/`-w` and `--workspaces`/`-ws` CLI flags, e.g. `npm run -w PACKAGE run lint`. The first Node version that ships with npm `v7.7.x` is [`v15.13.0`][node-dist-v15.13.0] which is globally available as of april 1, 2021 (no, it‘s not an april fool :smile:). To ensure that these constraints are met, without only relying on users to read the documentation, both `npm` and `node` have been added to [the `engines` field][npm-docs-pkgjson#engines] of the `package.json` file.

This change also comes with updates to all documentations, including the addition of the version constraints, as well as updates to repository template features like the GitHub Action workflows. The `.yarnrc` file has been replaced by `.npmrc` that includes the `package-lock=false` and `save-exact=false` configurations.

</details>

## Tasks

<details>
<summary><strong>Node package dependency & GitHub action version updates</strong> — #67, #68, #69, #74 ⇄ #75</summary>

↠ Bumped outdated Go module dependencies and GitHub actions to their latest versions:

- #67, #68, #69 (⊶ 8bff7a5d, 46a92f0a, 14a9108e) [`actions/setup-node`][gh-actions/setup-node] from [v2.1.2 to v2.1.5][gh-actions/setup-node-comp-v2.1.2_v2.1.5]
- #74 ⇄ #75 (⊶ d827dbf5) [husky][] — Bumped minimum version from [`v4.3.0` to `v6.0.0`][gh-typicode/husky-comp-v4.3.0_v6.0.0]. This also includes some breaking changes that require migrations. Run the official migration CLI to automatically migrate from v4 to v6: `npx husky-init && npm exec -- github:typicode/husky-4-to-6 --remove-v4-config`
- #74 ⇄ #75 (⊶ d827dbf5) [lint-staged][] — Bumped minimum version from [`v10.5.1` to `v10.5.4`][gh-okonet/lint-staged-comp-v10.5.1_v10.5.4].
- #74 ⇄ #75 (⊶ d827dbf5) [remark-cli][gh-remarkjs/remark] — Bumped minimum version from [`v8.0.1` to `v9.0.0`][gh-remarkjs/remark-comp-v8.0.1_v9.0.0].

</details>

<details>
<summary><strong>Dependency handling with lockfiles</strong> — #70 ⇄ #71 (⊶ a98d9b1c)</summary>

↠ The usage of dependency lockfiles like [`package-lock.json`][npm-docs-v7-lockfile] or [`yarn.lock`][yarn-docs-lock] has always been a controversial topic where opinions go in different directions. On one side many project maintainers tend to argue that is helps to achieve deterministic build results, but on the side it might also hide problems when any later versions of a used dependency, or its transitive dependencies, is not compatible with the own project anymore.

I‘ve investigated a lot of time into research again to finally find a solution that works for my projects. In short, the result is to go with the rule that is also used by many large-scale projects: Do **not use lockfiles for multi-consumer projects like libraries** but **only for single-consumer projects like applications**.

Therefore the `yarn.lock` file has been removed since this makes no sense for a repository template anyway. See the sections below for some more details about how to decide to use a lockfile or not.

### When to use lockfiles

The clear advantage of lockfiles are reproducible builds and the persistence of a running project state. They ensure that a project artifact can be rebuild at anytime using the exact same dependencies, resulting in the exact same artifact, even when the project was not updated in years.
This applies to projects that are focused on building a **end-to-end experience like applications and other end-user products**.

These are the advantages listed in the official npm documentation about `package-lock.json` files:

> - Describe a single representation of a dependency tree such that teammates, deployments, and continuous integration are guaranteed to install exactly the same dependencies.
> - Provide a facility for users to "time-travel" to previous states of node_modules without having to commit the directory itself.
> - Facilitate greater visibility of tree changes through readable source control diffs.
> - Optimize the installation process by allowing npm to skip repeated metadata resolutions for previously-installed packages.
> - As of npm `v7`, lockfiles include enough information to gain a complete picture of the package tree, reducing the need to read package.json files, and allowing for significant performance improvements.

Like mentioned, npm `v7` comes with a lot of advantages and the [team recommends to commit the file into project repositories][npm-blog-v7_keep_lock]:

- the lockfile has enough information to describe the precise package tree all by itself.
- the lockfile maps the packages to their information by their relative location to the root (instead of their name).
- the npm CLI uses `yarn.lock` lockfiles if available, as a source of package metadata and resolution guidance when there is missing information, knowing that the `package-lock.json` is the authoritative definition.
  - `yarn.lock` lockfiles cannot completely replace npm’s lockfile since the current implementation doesn’t have enough information needed for the complete npm functionality.
- the npm CLI uses a “hidden lockfile“ placed inside the `node_module` directory that helps to avoid repeated package tree reading.

Another point is that in end-user projects dependencies in `package.json` files are often pinned instead of using [SemVer range selectors][npm-webapp-semver] like `^` (latest minor-only) or `~` (latest patch only). In such cases a lockfile helps to keep control about transitive dependencies and persist projects states in time.

### When to avoid lockfiles

Even though [the Yarn team published a blog post in 2016][yarn-blog-lockfiles] that states to always commit the `yarn.lock` file, regardless of the project type, this advice was not adopted by every project and some “real-world scenarios“ often showed that this decision was justified. There are [blog posts that summarize when not to use a lockfile][devto-gajus-stop_lockfile] where even [Yarn maintainers reply with comments that claim the opposite][devto-gajus-stop_lockfile-comment-yarn_maintainer], but over the time more and more projects went away from using lockfiles.
One argument is that [lockfiles are important to enure that library contributors in 10 years still know what was the last confirmed set of packages which worked as expected][tw-arcanis-1164229994165559299-comment-19], but this can almost be ignored in a ecosystem like Node that changes almost every day.

Another important point is to mention that the usage of [lockfiles were also a attack surface to inject malicious dependencies][snyk-blog-lockfile]. Due to the large size of lockfiles, it is also often a challenge for project maintainers to review and validate a lockfile in pull requests are so they are often [ignored and blindly trusted][tw-bcrypt-1208950722097598465].

The community is still not of one opinion and I guess this will never change, but [learning about the experience of well-known maintainers][gh-sindresorhus/ama-479#c-310661514] and [popular projects][gh-airbnb/javascript-2409] is often a good way to find the own decision.

In conclusion, the usage of lockfiles in a non-end-user project can be well summarized with [“just postponing the inevitable breakage“][tw-renovatebot-1163789817492230144]:

<p align="center"><img src="https://user-images.githubusercontent.com/13448100/113289644-c3388680-92f0-11eb-9a0b-d710c78edb92.png" width="350" /></p>

</details>

# 0.8.0

![Release Date: 2020-12-12](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2020-12-12&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.8.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/11) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.8.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/8)

⇅ [Show all commits][gh-compare-tag-v0.7.0_v0.8.0]

## Improvements

<details>
<summary><strong>Reduce Dependabot PR noise for NPM package ecosystem</strong> — #65 ⇄ #66 (⊶ 32925a1f)</summary>

↠ To reduce the noise of too many PRs from NPM dependencies, where most of them are only scoped for (local) development, two optimizations have been made:

1. The schedule changed to the [`monthly` interval][gh-docs-dep_updates#schedule_interval]. This is still enough to keep up with the fast updates in the NPM ecosystem.
2. Only watch **production** packages (`dependencies`) and **ignore development packages** (`devDependencies`). The packages used for local or CI/CD development purposes are not required to be the latest version just for the sake of being up-to-date without a specific need or benefit.

Since [GitHub takes security really serious][gh-feat-sec], important Dependabot [security updates][gh-docs-dep_sec_updates] are triggered manually by a security advisor so there is no risk of missing important versions bumps when reducing the schedule interval.

> Use the `allow` option to customize which dependencies are updated. This has no impact on security updates for vulnerable dependencies.

</details>

## Tasks

<details>
<summary><strong>Prepared project/repository publication</strong> — #59 ⇄ #60 (⊶ 50238339)</summary>

↠ Before switching the [GitHub repository visibility][gh-docs-repo_vis] to “public“ a few adjustments had to be made.
Basically #22 was reverted, taking the changes from #23 into account, so that SVG images like the repository hero are using the URLs for public repositories again instead of the ones that allow to resolve the files in private repositories.

</details>

<details>
<summary><strong>Node.js package dependency version updates</strong> — #63</summary>

↠ Bumped outdated Node.js package dependencies to their latest versions:

- #63 (⊶ 35287545) [`prettier`][gh-prettier/prettier] from [2.1.2 to 2.2.1][gh-prettier/prettier-comp-v2.1.2_v2.2.1] — The the [official Prettier 2.2 introduction blog post][prettier-blog-rl_2.2] for more details.

</details>

# 0.7.0

![Release Date: 2020-11-09](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2020-11-09&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.7.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/10) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.7.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/7)

⇅ [Show all commits][gh-compare-tag-v0.6.0_v0.7.0]

## Tasks

<details>
<summary><strong>Updated to latest Node.js package dependency & GitHub Action versions</strong> — #54,#55,#56,#57</summary>

↠ Bumped outdated Node.js package dependencies & GitHub Actions to their latest versions:

- #54 (⊶ 41e11b94) [`actions/setup-node`][gh-actions/setup-node] from [v1 to v2.1.2][gh-actions/setup-node-comp-v1_v2.1.2] — v2 comes with speed and performance improvements by using a dedicated, built-in image cache for Node.js distributions.
- #55 (⊶ d1e3d538) [`lint-staged`][lint-staged] from [10.2.11 to 10.5.12][gh-lint-staged-comp-v10.2.11_v10.5.1]
- #56 (⊶ 22652c51) [`husky`][husky] from [4.2.5 to 4.3.0][gh-husky-comp-v4.2.5_v4.3.0]
- #57 (⊶ 17378a12) [`prettier`][prettier] from [2.0.5 to 2.1.2][gh-prettier-comp-2.0.5_2.1.2]

</details>

# 0.6.0

![Release Date: 2020-11-08](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2020-11-08&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.6.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/9) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.6.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/6)

⇅ [Show all commits][gh-compare-tag-v0.5.0_v0.6.0]

## Feature

<details>
<summary><strong>Initial project documentation</strong> — #50 ⇄ #51 (⊶ f18bf434)</summary>

↠ Wrote the initial project documentation for the `README.md` file that includes…

1. …an project introduction and motivation.
2. …an overview of the project features.
3. …a listing of available template repositories.
4. …a rough overview of the directory structure.
5. …more detailed sections about all features.
6. …some basic instructions how to use this template repository.
7. …information about how to contribute to this project.

</details>

<details>
<summary><strong>Dependabot: Automated dependency updates and security alerts</strong> — #52 ⇄ #53 (⊶ 4816e4ec)</summary>

↠ In June 2020 [Dependabot][] was [natively integrated into GitHub][gh-blog-dependabot]. This allows to use [automated dependency updates][gh-docs-dependabot] and [security vulnerability alerts][gh-docs-sec_vuls-alerts].

Created the [`dependabot.yml` file][gh-docs-sec_vuls-dependabot_config] and configured updates for GitHub Actions and Yarn/NPM. The documentation also mentions the need to [manually enable or disable Dependabot per repository][gh-docs-dependabot_activation].

</details>

# 0.5.0

![Release Date: 2020-10-31](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2020-10-31&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.5.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/8) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.5.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/5)

⇅ [Show all commits][gh-compare-tag-v0.4.0_v0.5.0]

## Improvements

<details>
<summary><strong>Use namespace for NPM package name</strong> — #48 ⇄ #49 (⊶ 85f0b08c)</summary>

↠ To prevent collisions with already existing NPM packages like [tmpl][npm-tmpl] the NPM package name of this repository has been changed to use the `@svengreb` namespace prefix.

</details>

# 0.4.0

![Release Date: 2020-09-25](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2020-09-25&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.4.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/7) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.4.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/4)

⇅ [Show all commits][gh-compare-tag-v0.3.0_v0.4.0]

## Improvements

<details>
<summary><strong>Optimize OS version matrix strategy for CI workflow</strong> — #46 ⇄ #47 (⊶ cb772586)</summary>

↠ Before the CI workflow used a matrix strategy to run the `lint-node` job, but this was not necessary for this repository. It has been improved to make the workflow run faster by avoiding unnecessary steps. The `lint-node` job has been changed to only run on the [currently latest stable Node version `14.x`][gh-nodejs/node-blob-cl-v14] only on _Linux_ because this repository is not focused on JavaScript but only runs Node based tools to lint other files within this repository.

This change also helps to keep the required GitHub Action run minutes for the account of this repository as small as possible without wasting resources for unnecessary tasks.

</details>

# 0.3.0

![Release Date: 2020-09-20](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2020-09-20&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.3.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/6) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.3.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/3)

⇅ [Show all commits][gh-compare-tag-v0.2.0_v0.3.0]

## Improvements

<details>
<summary><strong>Pin version to v2 for actions/checkout in CI workflow</strong> — #44 ⇄ #45 (⊶ 5d2ce0c0)</summary>

↠ Before [`actions/checkout` was used from the `master` branch][repo-blob-ci.yml-e53fbaf8#l31] in the [`ci` workflow][repo-actions-query-ci]. This has now ben pinned to the latest version `v2` to ensure a stable pipeline.

</details>

# 0.2.0

![Release Date: 2020-09-20](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2020-09-20&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.2.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/5) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.2.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/2)

⇅ [Show all commits][gh-compare-tag-v0.1.0_v0.2.0]

## Features

<details>
<summary><strong>Git ignore pattern for Yarn and JetBrains products</strong> — #39 ⇄ #40 (⊶ 5d2ce0c0)</summary>

↠ Before there were only ignore pattern for the Node.js `node_modules` folder, but specific pattern for [Yarn][] were missing.
Because the fantastic [JetBrains products][jetbrains] like [GoLand][] (or respectively [IntelliJ][] with the [official Go plugin][jetbrains-plugins]) are an integral part of my daily toolbox the pattern have also been added.

</details>

## Improvements

<details>
<summary><strong>More fine-grained GitHub Action trigger configuration</strong> — #34 ⇄ #35 (⊶ c184ae8b)</summary>

↠ The _CI_ GitHub Action Workflow used the `push` setting for the `on` field in order to trigger the workflow. That was superficial and the workflow ran for every new commit and PR.

To improve this behavior and prevent unnecessary workflow runs the [`push` field][gh-docs-actions-wf#on_push_pull] of [the `on` field][gh-docs-actions-wf#on_push_pull] is now configured to only run for the `main` branch and `v*` tags while [the `pull_request` field][gh-docs-actions-wf#on_pull] is set without any specific configuration so that it runs for all PRs regardless of the target branch.

</details>

<details>
<summary><strong>Lowercase naming for GitHub Action workflows</strong> — #36 ⇄ #37 (⊶ e53fbaf8)</summary>

↠ Even though it is possible to use uppercase names (including whitespaces) for GitHub Action workflows it is best practice for almost every language to use lowercase names. This prevents problems with parsing as well as errors due to lower- and uppercase mismatches.

One example is the [shields.io][] SVG badge that is used in the README of this repository: The actual workflow is only found when the name matches the exact notation including lower- and uppercase characters
To mitigate such problems the name has been changed to lowercase only.

</details>

<details>
<summary><strong>Maximum line length for Markdown EditorConfig</strong> — #41 ⇄ #42 (⊶ 6c1a2ac9)</summary>

↠ Since _Markdown_ is written as flowing text the globally defined maximum line length of EditorConfig has been disabled to prevent false-positive errors.

</details>

## Bug Fixes

<details>
<summary><strong>Fix missing LF EOL definition in Git attributes</strong> — #38 ⇄ #43 (⊶ 01d4aee3)</summary>

↠ The comment for the `* text=auto` rule in the `.gitattributes` file documents the usage of LF for EOL, but the actual `eol=lf` property was missing.

</details>

# 0.1.0

![Release Date: 2020-08-26](https://img.shields.io/static/v1?style=flat-square&label=Release%20Date&message=2020-08-26&colorA=4c566a&colorB=88c0d0) [![Project Board](https://img.shields.io/static/v1?style=flat-square&label=Project%20Board&message=0.1.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/projects/4) [![Milestone](https://img.shields.io/static/v1?style=flat-square&label=Milestone&message=0.1.0&logo=github&logoColor=eceff4&colorA=4c566a&colorB=88c0d0)](https://github.com/svengreb/tmpl/milestone/1)

⇅ [Show all commits][gh-compare-tag-init_v0.1.0]

This is the initial release version of _tmpl_.
It represents the basic project setup, structure and development workflow. There are many initial configurations and documentations that are covered in the following sections of this version changelog to introduce used technologies and explain why several decisions have been made.

## Features

<details>
<summary><strong>Initial repository README and assets</strong> — #1,#21/#22,#23 (⊶ 0b0d7aee)</summary>

↠ The `main` branch stores documentations and assets about the
This introduces the `README.md` with basic content as well as the projects logo and repository hero assets.
It will be extended later on with more project information and usage
instruction for the different template repositories.

</details>

<details>
<summary><strong>Yarn and NPM package configuration</strong> — #2 (⊶ a1788097)</summary>

<!-- sources: https://upload.wikimedia.org/wikipedia/commons/d/db/Npm-logo.svg, https://yarnpkg.com -->

<p align="center"><img src="https://user-images.githubusercontent.com/13448100/85054959-2069ae00-b19d-11ea-9023-bd4de32918a6.png" width="222" /> <img src="https://user-images.githubusercontent.com/13448100/85054952-1d6ebd80-b19d-11ea-95a2-d51011178294.png" width="180" /></p>

↠ Created the [`package.json`][npm-docs-pkg] and [`.yarnrc`][yarn-docs-rc] configuration files to declare and setup [NodeJS][] tools. This includes the usage of the [Yarn][yarn-docs-config] `exact` version resolution. Since _Yarn_ is used npm's [`package-lock.json`][npm-docs-lockfile] file will never be created and tracked in order to prevent conflicts with the [`yarn.lock`][yarn-docs-lock] file.

</details>

</details>

<details>
<summary><strong>MIT license</strong> — #3 ⇄ #4 (⊶ 1ec34446)</summary>

<p align="center"><img src="https://user-images.githubusercontent.com/7836623/63597693-57dbee00-c5be-11e9-81a2-d6632f8d81bb.png" width="180" /></p>

↠ Added the `LICENSE` file for the [MIT license][mit]. This includes the `main` branch since the file covers the content of the repository itself and is also part of the _base_ template.

</details>

<details>
<summary><strong>Git ignore and attribute pattern</strong> — #5,#24 ⇄ #6,#25 (⊶ cd509de1)</summary>

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/e/e0/Git-logo.svg" width="266" /></p>
<!-- PNG format: https://user-images.githubusercontent.com/7836623/63598175-56f78c00-c5bf-11e9-9cab-d13644dd4454.png -->

↠ Added the [`.gitattributes`][git-docs-attr] and [`.gitignore`][git-docs-ignore] configuration files to define the pattern.

</details>

<details>
<summary><strong>Git mail mapping</strong> — #7 ⇄ #8 (⊶ )</summary>

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/e/e0/Git-logo.svg" width="266" /></p>
<!-- PNG format: https://user-images.githubusercontent.com/7836623/63598175-56f78c00-c5bf-11e9-9cab-d13644dd4454.png -->

↠ Added a Git [mailmap][git-docs-mailmap] file to link to in documentations to allow contributors to send mails regarding security issues. This prevents unnecessary overhead of updating all documents when new core team and members and contributors are added and additionally adds the main functionality of the file: Mapping commits when someone changed the email address or uses a different one.

</details>

<details>
<summary><strong>Git mail mapping</strong> — #7 ⇄ #8 (⊶ 68a9ee81)</summary>

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/e/e0/Git-logo.svg" width="266" /></p>
<!-- PNG format: https://user-images.githubusercontent.com/7836623/63598175-56f78c00-c5bf-11e9-9cab-d13644dd4454.png -->

↠ Added a Git [mailmap][git-docs-mailmap] file to link to in documentations to allow contributors to send mails regarding security issues. This prevents unnecessary overhead of updating all documents when new core team and members and contributors are added and additionally adds the main functionality of the file: Mapping commits when someone changed the email address or uses a different one.

</details>

<details>
<summary><strong>GitHub code owners</strong> — #9 ⇄ #10 (⊶ 960c939f)</summary>

<p align="center">
  <img src="https://user-images.githubusercontent.com/7836623/63598769-85c23200-c5c0-11e9-967e-c8b3e5b43458.png" width="160" />
</p>

↠ Adapted GitHub's [code owners][gh-blog-codeowner_intro] feature. This allows to define matching pattern for project paths to automatically add all required reviewers of the core team and contributors to new PRs.

See [GitHub documentation][gh-docs-repos-codeowner] for more details.

<p>
  <figure>
    <div align="center">
      <img src="https://user-images.githubusercontent.com/7836623/63598793-91adf400-c5c0-11e9-99f8-2feaeaf57bd3.png" />
      <figcaption>Sidebar for <em>code owner</em> PR review requests and review stats</figcaption>
    </div>
  </figure>
</p>

<p>
  <figure>
    <div align="center">
      <img src="https://user-images.githubusercontent.com/7836623/63599279-8effce80-c5c1-11e9-9b87-1e1c276f7c6d.png" />
      <figcaption>Branch protection configuration to enable required <em>code owner</em> review approvals</figcaption>
    </div>
  </figure>
</p>

<p>
  <figure>
    <div align="center">
      <img src="https://user-images.githubusercontent.com/2513/27803610-544ba222-5ff8-11e7-9313-e4062315fb0c.png" />
      <figcaption>PR status checks when required <em>code owner</em> review is pending</figcaption>
    </div>
  </figure>
</p>

</details>

<details>
<summary><strong>EditorConfig</strong> — #11 ⇄ #12 (⊶ 0745977d)</summary>

<p align="center"><img src="https://user-images.githubusercontent.com/7836623/63612291-37239080-c5de-11e9-9b28-f15465083101.png" /></p>
<!-- Source: https://editorconfig.org/logo.png -->

↠ Added the [EditorConfig][] file to define and maintain consistent coding styles between different editors and IDEs.

</details>

<details>
<summary><strong>Code Formatter: Prettier</strong> — #13 ⇄ #14 (⊶ 196cdf2e)</summary>

<p align="center"><img src="https://user-images.githubusercontent.com/7836623/63637792-4dcef380-c681-11e9-9252-f2fb22499985.png" width="266" /></p>

↠ A code formatter is a essential part of a project setup to ensure a good and consistent code style without requiring relatively time-consuming manual corrections found by a code linter. With code being automatically formatted on actions like saving a file the developer can focus entirely on the code instead of spending time and energy on indenting code line by line.

That‘s where one special project comes in: [Prettier][], the opinionated code formatter with support for almost any language and integration with almost every popular editor. I‘ve been using it since the first version and I totally forgot about the fact that formatting is even a thing. That could also be because [Gophers][go-blog-gopher] are already used to [`gofmt`][gofmt] anyway.

_Prettier_ is a absolute must-have for every project setup and I‘m not aware of any other projects with such advanced parsers and language support. The only negative point is that it is written in _JavaScript_ instead of [Go][] so it always pulls in [NodeJS][] as a development dependency. This is not a problem at all for web-based projects, but for _Go_ or any other non-NodeJS project it inflates the setup unnecessarily.

Anyway, the fantastic developer experience and project benefits clearly outweigh the negative points. In addition many developers today already have _Node_ installed locally since it‘s large ecosystem has already spread by far further than just the web but already powers many system, desktop and CLI applications.

### Configuration

This is one of the main features of Prettier: It already provides the best and recommended style configurations of-out-the-box™.
The only option that was changed is the [print width][prettier-docs-pwidth]. It is set to `80` by default which is not up-to-date for modern screens (might only be relevant when working in terminals only like e.g. with _Vim_) and has therefore been changed to `120` like defined in [all of my style guides][gh-stg-repos].
The `prettier.config.js` configuration file is placed in the project root as well as the `.prettierignore` file to also define ignore pattern.

### Package Script

To allow to format all sources a `format:pretty` package script has been added that also runs in the main `format` script flow.
The new `lint:pretty` script allows to check if all supported files are formatted correctly. It is included in the main `lint` script flow.

</details>

<details>
<summary><strong>Markdown Linting: remark-lint</strong> — #15 ⇄ #16 (⊶ 552dded0)</summary>

<p align="center"><img src="https://raw.githubusercontent.com/remarkjs/remark-lint/02295bc/logo.svg?sanitize=true" width="180" /></p>
<!-- PNG format: https://user-images.githubusercontent.com/7836623/63633312-ce243300-c646-11e9-818a-cef1223b3490.png -->

↠ Ensuring that documentations and content written in [Markdown][] are of great quality should be a continuous goal of any project. Persisting information is a consistent and extensive way helps tp keep a project healthy, no matter whether for long-time or new users ad well as maintainers and contributors. Linting _Markdown_ results in better rendering with different markdown parsers and makes sure less refactoring is needed afterwards.

The best solution for this task would be a tool written in [Go][], but the undisputed best tool is still [remark-lint][] which is (unfortunately) written in _JavaScript_ and used via [NodeJS][]. Of course there are fantastic projects written in _Go_ like [goldmark][] that provides a great way to _parse_ content, but linting is (currently) not a feature.
_remark-lint_ on the other side is built on top of [remark][], the powerful Markdown processor powered by plugins (such as _remark-lint_ itself) and part of the [unifiedjs][] collective. It comes with really [large set of customizable rule][gh-remark-lint-rules] and ways to ensure _Markdown_ will be consistent across any project.

Another decision point for _remark-lint_ was the fact that _Prettier_ has been added in #13 to this template so _Node_ is already a development dependency anyway. This also allows to add other awesome projects that are (unfortunately) written in _JavaScript_ and for which there is no comparable alternative.

### Configuration

_remark-lint_ can be used via [remark-cli][npm-remark-cli] and a rule preset. This preset is [remark-preset-lint-arcticicestudio][gh-remark-lint-preset], my custom preset that implements my [Markdown Style Guide][gh-stg-md].
Since the custom preset is still in major version `0` the version range should be `>=0.x.x <1.0.0` to avoid the “SemVer Major Zero Caveat”. When defining package versions with the the carat `^` or tilde `~` range selector it won‘t affect packages with a major version of `0`. _Yarn_ will resolve these packages to their exact version until the major version is greater or equal to `1`.
To avoid this caveat the more detailed version range `>=0.x.x <1.0.0` should be used to resolve all versions greater or equal to `0.x.x` but less than `1.0.0`. This will always use the latest `0.x.x` version and removes the need to increment the version manually on each new release.

The `.remarkrc.js` configuration file is placed in the project root as well as the `.remarkignore` file to also define ignore pattern.

### Package Script

To allow to run the Markdown linting separately a `lint:md` package script has been added and included in the main `lint` script flow.

<p align="center"><img src="https://user-images.githubusercontent.com/7836623/63633316-ded4a900-c646-11e9-9f55-fbebb8f5842b.png" width="622" /></p>

</details>

<details>
<summary><strong>Git Hook: lint-staged</strong> — #17 ⇄ #18 (⊶ abae9c23)</summary>

<p align="center"><img src="https://user-images.githubusercontent.com/7836623/63638143-c84d4280-c684-11e9-93cf-98662c6c0168.png" width="180" /></p>

↠ [Git Hooks][git-docs-hooks] are a fantastic way to customize the development workflow of a project to simplify and automate specific tasks that are required when working on the code base. For example, this includes tasks like formatting, _linting_ and running tests before pushing a commit to ensure it conforms to the code style and works as expected.
Most _Git Hooks_ are not that complex and fullfil a simple purpose while other solutions like [Danger][] can help to manage larger projects and projects that need to scale.

This _base_ template repository will initially use a _Git Hook_ that automatically runs configured _linters_ on all files that have been _staged_ and that match the configured pattern (file extension, filename etc.).
Like documented in #15, [NodeJS][] is already a development dependency anyway so the [lint-staged][] NPM package will be used for this goal. I‘ve used this package in almost any project and it‘s again the most stable, production-proven and advanced tool that is currently out there with no comparable alternatives in other languages.

<p align="center"><a href="https://asciinema.org/a/199934"><img src="https://asciinema.org/a/199934.svg" width="600" /></a></p>

### Configuration

The configuration file `lint-staged.config.js` is placed in the project root and includes the command that runs for matching file extensions (globs). It includes at least the three following entries with the same order as listed here:

1. `prettier --list-different` - Runs [Prettier][] (#13) to ensure all files are formatted correctly. The `--check` prints files that are not conform with the _Prettier_ configuration.
2. `remark --no-stdout` - Runs [remark-lint][] (#15) against `*.md` to ensure all Markdown files are compliant to the style guide. The `--no-stdout` flag suppresses the output of the parsed file content.

</details>

<details>
<summary><strong>Git Hook Manager & Runner: husky</strong> — #19 ⇄ #20 (⊶ 49d1fac0)</summary>

<p align="center"><img src="https://user-images.githubusercontent.com/7836623/63638276-5970e900-c686-11e9-9de8-a54fc0a75b1b.png" width="180" /></p>

> Git hooks made easy :dog: woof!

↠ In #17 [lint-staged][] was added, a [Git Hook][git-docs-hooks] runner that runs _linters_ against _staged_ files before each commit.
To automatically run this and other hooks that might be added later on, the hook manager and runner [husky][] is used.

Just like other already added tools ([Prettier][gh-13], [remark-lint][gh-15], [lint-staged][gh-17]), _husky_ is (unfortunately) also written in _JavaScript_. Since [NodeJS][] is therefore already a development dependency it doesn‘t really matter that _husky_ is another NPM package too.
Unlike these previous tools there are indeed alternatives written in [Go][] like [lefthook][] or [quickhook][], but it requires time to test and evaluate them before actually replacing _husky_. Also a long as there are no comparable alternatives to the already used tools listed above, this template would be more complex by requiring both _Node_ and _Go_ as development dependency. Therefore _husky_ will take over the part as hook manager & runner since it is a stable, production-proven and advanced project that I already use in almost any other project setup.

### Configuration

The `.huskyrc.js` configuration file is placed in the project root and includes the command to run for any [supported Git hook][husky-hooks]. Initially it contains an entry for the following hook:

- `pre-commit` - Runs _lint-staged_ (#17) before each commit to ensure all staged files are compliant to all style guides.

</details>

<details>
<summary><strong>Git Hook Manager & Runner: husky</strong> — #19 ⇄ #20 (⊶ 49d1fac0)</summary>

<p align="center"><img src="https://user-images.githubusercontent.com/7836623/63638276-5970e900-c686-11e9-9de8-a54fc0a75b1b.png" width="180" /></p>

> Git hooks made easy :dog: woof!

↠ In #17 [lint-staged][] was added, a [Git Hook][git-docs-hooks] runner that runs _linters_ against _staged_ files before each commit.
To automatically run this and other hooks that might be added later on, the hook manager and runner [husky][] is used.

Just like other already added tools ([Prettier][gh-13], [remark-lint][gh-15], [lint-staged][gh-17]), _husky_ is (unfortunately) also written in _JavaScript_. Since [NodeJS][] is therefore already a development dependency it doesn‘t really matter that _husky_ is another NPM package too.
Unlike these previous tools there are indeed alternatives written in [Go][] like [lefthook][] or [quickhook][], but it requires time to test and evaluate them before actually replacing _husky_. Also a long as there are no comparable alternatives to the already used tools listed above, this template would be more complex by requiring both _Node_ and _Go_ as development dependency. Therefore _husky_ will take over the part as hook manager & runner since it is a stable, production-proven and advanced project that I already use in almost any other project setup.

### Configuration

The `.huskyrc.js` configuration file is placed in the project root and includes the command to run for any [supported Git hook][husky-hooks]. Initially it contains an entry for the following hook:

- `pre-commit` - Runs _lint-staged_ (#17) before each commit to ensure all staged files are compliant to all style guides.

</details>

<details>
<summary><strong>GitHub "CI" Action Workflow</strong> — #28 ⇄ #29 (⊶ b4e3c396)</summary>

<p align="center"><img src="https://user-images.githubusercontent.com/13448100/91175894-c4159400-e6e1-11ea-893d-84c1b561fe31.png" /></p>

↠ Using [GitHub Actions][gh-docs-actions] brings many advantages like a „close-to-the-source“ development pipeline. Having the code and automated pipelines/workflows in one place is worth a lot. This also comes along with the perfect and smooth integrations into GitHub platform and page itself like status reports on PRs and many more possibilities like the [customization and triggering of workflows through webhooks][gh-docs-actions-events] for almost every event that can occur in a repository/issue/PR etc.

<p align="center">
  <figure>
      <div align="center"><img src="https://user-images.githubusercontent.com/13448100/91175909-c8da4800-e6e1-11ea-9efa-800c82154e2e.jpg" width="712" /></div>
      <figcaption><div align="center">The <em>GitHub Actions</em> CI/CD UI</div></figcaption>
  </figure>
</p>

**See the [official GitHub Actions documentation][gh-docs-actions] for details about setups, features, the configuration API and many more!**

<p align="center">
  <figure>
      <div align="center"><img src="https://user-images.githubusercontent.com/13448100/91175904-c7a91b00-e6e1-11ea-9729-7e1c4fa1656f.gif" width="712" /></div>
      <figcaption><div align="center">Live logs showing real-time feedback</div></figcaption>
  </figure>
</p>

The initial implementation focuses on _continuous integration_ to the Node.js scripts with Yarn that are defined in the `package.json` file.

There is official support for SVG badges, but in order to use the same custom configurations and style of already included badges the ones provided by [shields.io][] are used.

</details>

<details>
<summary><strong>GitHub Open Source community standards</strong> — #30 ⇄ #31 (⊶ 9495d3cc)</summary>

<p align="center"><img src="https://opensource.guide/assets/images/illos/beginners.svg?sanitize=true" width="360" /></p>
<!-- PNG format: https://user-images.githubusercontent.com/7836623/63600050-1b5ec100-c5c3-11e9-935e-ee8e2f3d7b49.png -->

↠ GitHub introduced a feature for [recommended community standards][gh-blog-community_tools]. _tmpl_ tries to adhere to the great [Open Source Guides](https://opensource.guide) and adapted to the recommendations to complete the projects [community profile][gh-comm_profile].

<p align="center"><img src="https://user-images.githubusercontent.com/13448100/91279813-42c40d00-e786-11ea-929c-cac108cdd8aa.png" width="622" /></p>

### Code of Conduct

To facilitate a healthy and constructive community behavior, _tmpl_ adheres and enforces a [code of conduct][oss-guides-coc].

<p align="center"><img src="https://opensource.guide/assets/images/illos/coc.svg?sanitize=true" width="360" /></p>
<!-- PNG format: https://user-images.githubusercontent.com/7836623/63600052-1b5ec100-c5c3-11e9-9124-e5ebbe8cee5b.png -->

It includes sections about

- what we believe in and how we act
- unacceptable behavior
- the responsibilities of the maintainer
- the enforcement of the Code of Conduct
- consequences for violations
- the scope of the Code of Conduct

See the [GitHub documentation][gh-docs-coc] for more details about the provided integrations.

### Contributing Guidelines

The [contribution guidelines][gh-blog-contrib_guidelines] help to build a community that [encourages people to use, contribute to][oss-guides-contrib], and evangelize a project.

<p align="center"><img src="https://opensource.guide/assets/images/illos/contribute.svg?sanitize=true" width="40%" /></p>
<!-- PNG format: https://user-images.githubusercontent.com/7836623/63600053-1b5ec100-c5c3-11e9-8f5d-a8a824df60b4.png -->

It will include sections about

- how to get started
- bug reports
- enhancement suggestions
- pull requests
- style guides
  - [JavaScript Style Guide][styleguide-js]
  - [Markdown Style Guide][styleguide-md]
  - [Git Style Guide][styleguide-git]
- credits

See the [GitHub introduction blog post][gh-blog-contrib_guidelines] and [GitHub documentation][gh-docs-contrib-guide] for more details about the provided integrations.

</details>

<details>
<summary><strong>GitHub issue and pull request templates</strong> — #32 ⇄ #33 (⊶ 0bc90483)</summary>

↠ GitHub provides a feature to define [multiple issue templates][gh-blog-issue_pr_tmpl_multi].

<p align="center"><img src="https://user-images.githubusercontent.com/13448100/91281927-edd5c600-e788-11ea-9da7-fb3210ef9bf8.png" width="712" /></p>

The [initial template file that has been used when the feature was introduced][gh-blog-issue_pr_tmpl_intro] can now be used as a fallback/generic template.

<p align="center"><img src="https://user-images.githubusercontent.com/13448100/91281936-f1694d00-e788-11ea-92f4-ad703ffdaae0.png" width="712" /></p>

The UI helps users to open a new issue in projects by prompting them to choose from multiple issue types.

<p align="center"><img src="https://user-images.githubusercontent.com/13448100/91281941-f3cba700-e788-11ea-823e-7b0e66ebded2.png" /></p>

See the [GitHub documentation][gh-docs-issue_pr_tmpl] for more details about issue and pull request templates. Also check out how to manually create [issue templates][gh-help-issue_tmpl] and a [pull request template][gh-docs-pr_tmpl]. There's also a guide on [how to create the (deprecated) fallback/generic issue template][gh-docs-issue_tmpl_manually].

</details>

## Improvements

<details>
<summary><strong>Improve per-file documentations</strong> — #26 ⇄ #27 (⊶ 2acd89ec)</summary>

↠ Some files contained comments with details about the file content as well as references to documentations and websites. These have been improved by simplifying the way how references are listed, removing unnecessary content as well as moving comments to the places in the file where they are relevant.

</details>

<!--
+------------------+
+ Formatting Notes +
+------------------+

The `<summary />` tag must be separated with a blank line from the actual item content paragraph,
otherwise Markdown elements are not parsed and rendered!

+------------------+
+ Symbol Reference +
+------------------+
↠ (U+21A0): Start of a log section description
— (U+2014): Separator between a log section title and the metadata
⇄ (U+21C4): Separator between a issue ID and pull request ID in a log metadata
⊶ (U+22B6): Icon prefix for the short commit SHA checksum in a log metadata
⇅ (U+21C5): Icon prefix for the link of the Git commit history comparison on GitHub
-->

<!--lint disable final-definition-->

<!-- Shared -->

[gh-actions/setup-node]: https://github.com/actions/setup-node
[gh-docs-actions]: https://docs.github.com/en/actions
[husky]: https://github.com/typicode/husky
[lint-staged]: https://github.com/okonet/lint-staged
[prettier]: https://prettier.io
[shields.io]: https://shields.io
[yarn-docs-lock]: https://yarnpkg.com/lang/en/docs/yarn-lock
[yarn]: https://yarnpkg.com

<!-- v0.1.0 -->

[danger]: https://danger.systems
[editorconfig]: https://editorconfig.org
[gh-13]: https://github.com/svengreb/tmpl/issues/13
[gh-15]: https://github.com/svengreb/tmpl/issues/15
[gh-17]: https://github.com/svengreb/tmpl/issues/17
[gh-blog-codeowner_intro]: https://github.blog/2017-07-06-introducing-code-owners
[gh-blog-community_tools]: https://github.blog/2017-06-14-new-community-tools
[gh-blog-contrib_guidelines]: https://github.blog/2012-09-17-contributing-guidelines
[gh-blog-issue_pr_tmpl_intro]: https://github.blog/2016-02-17-issue-and-pull-request-templates
[gh-blog-issue_pr_tmpl_multi]: https://github.blog/2018-01-25-multiple-issue-and-pull-request-templates
[gh-comm_profile]: https://github.com/svengreb/tmpl/community
[gh-compare-tag-init_v0.1.0]: https://github.com/svengreb/tmpl/compare/0b0d7aee...v0.1.0
[gh-docs-actions-events]: https://docs.github.com/en/actions/reference/events-that-trigger-workflows#webhook-events
[gh-docs-coc]: https://docs.github.com/en/github/building-a-strong-community/adding-a-code-of-conduct-to-your-project
[gh-docs-contrib-guide]: https://docs.github.com/en/github/building-a-strong-community/setting-guidelines-for-repository-contributors
[gh-docs-issue_pr_tmpl]: https://docs.github.com/en/github/building-a-strong-community/about-issue-and-pull-request-templates
[gh-docs-issue_tmpl_manually]: https://docs.github.com/en/github/building-a-strong-community/manually-creating-a-single-issue-template-for-your-repository
[gh-docs-pr_tmpl]: https://docs.github.com/en/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository
[gh-docs-repos-codeowner]: https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/about-code-owners
[gh-help-issue_tmpl]: https://help.github.com/articles/creating-issue-templates-for-your-repository
[gh-remark-lint-preset]: https://github.com/arcticicestudio/remark-preset-lint-arcticicestudio
[gh-remark-lint-rules]: https://github.com/remarkjs/remark-lint/blob/master/doc/rules.md
[gh-stg-md]: https://arcticicestudio.github.io/styleguide-markdown
[gh-stg-repos]: https://github.com/arcticicestudio?tab=repositories&q=styleguide
[git-docs-attr]: https://git-scm.com/docs/gitattributes
[git-docs-hooks]: https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks
[git-docs-ignore]: https://git-scm.com/docs/gitignore
[git-docs-mailmap]: https://git-scm.com/docs/git-shortlog#_mapping_authors
[go-blog-gopher]: https://blog.golang.org/gopher
[go]: https://go.dev
[gofmt]: https://golang.org/cmd/gofmt
[goldmark]: https://github.com/yuin/goldmard
[husky-hooks]: https://github.com/typicode/husky/blob/master/DOCS.md#supported-hooks
[lefthook]: https://github.com/Arkweid/lefthook
[markdown]: https://en.wikipedia.org/wiki/Markdown
[mit]: https://opensource.org/licenses/MIT
[nodejs]: https://nodejs.org
[npm-docs-lockfile]: https://docs.npmjs.com/files/package-lock.json
[npm-docs-pkg]: https://docs.npmjs.com/files/package.json
[npm-remark-cli]: https://www.npmjs.com/package/remark-cli
[oss-guides-coc]: https://opensource.guide/code-of-conduct
[oss-guides-contrib]: https://opensource.guide/how-to-contribute
[prettier-docs-pwidth]: https://prettier.io/docs/en/options.html#print-width
[quickhook]: https://github.com/dirk/quickhook
[remark-lint]: https://github.com/remarkjs/remark-lint
[remark]: https://remark.js.org
[styleguide-git]: https://github.com/arcticicestudio/styleguide-git
[styleguide-js]: https://github.com/arcticicestudio/styleguide-javascript
[styleguide-md]: https://github.com/arcticicestudio/styleguide-markdown
[unifiedjs]: https://unifiedjs.com
[yarn-docs-config]: https://yarnpkg.com/lang/en/docs/cli/config
[yarn-docs-rc]: https://yarnpkg.com/lang/en/docs/yarnrc

<!-- v0.2.0 -->

[gh-compare-tag-v0.1.0_v0.2.0]: https://github.com/svengreb/tmpl/compare/v0.1.0...v0.2.0
[gh-docs-actions-wf#on_pull]: https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestpaths
[gh-docs-actions-wf#on_push_pull]: https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestbranchestags
[goland]: https://www.jetbrains.com/go
[intellij]: https://www.jetbrains.com/idea
[jetbrains-plugins]: https://plugins.jetbrains.com
[jetbrains]: https://www.jetbrains.com

<!-- v0.3.0 -->

[gh-compare-tag-v0.2.0_v0.3.0]: https://github.com/svengreb/tmpl/compare/v0.2.0...v0.3.0
[repo-actions-query-ci]: https://github.com/svengreb/tmpl/actions?query=workflow%3Aci
[repo-blob-ci.yml-e53fbaf8#l31]: https://github.com/svengreb/tmpl/blob/e53fbaf8ff974a7c61d1ff51602175c82a35b20e/.github/workflows/ci.yml#L31

<!-- v0.4.0 -->

[gh-compare-tag-v0.3.0_v0.4.0]: https://github.com/svengreb/tmpl/compare/v0.3.0...v0.4.0
[gh-nodejs/node-blob-cl-v14]: https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V14.md

<!-- v0.5.0 -->

[gh-compare-tag-v0.4.0_v0.5.0]: https://github.com/svengreb/tmpl/compare/v0.4.0...v0.5.0
[npm-tmpl]: https://www.npmjs.com/package/tmpl

<!-- v0.6.0 -->

[dependabot]: https://dependabot.com
[gh-blog-dependabot]: https://github.blog/2020-06-01-keep-all-your-packages-up-to-date-with-dependabot
[gh-compare-tag-v0.5.0_v0.6.0]: https://github.com/svengreb/tmpl/compare/v0.5.0...v0.6.0
[gh-docs-dependabot_activation]: https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/enabling-and-disabling-version-updates
[gh-docs-dependabot]: https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/configuration-options-for-dependency-updates
[gh-docs-sec_vuls-alerts]: https://docs.github.com/en/free-pro-team@latest/github/managing-security-vulnerabilities/about-alerts-for-vulnerable-dependencies
[gh-docs-sec_vuls-dependabot_config]: https://docs.github.com/en/free-pro-team@latest/github/managing-security-vulnerabilitiesconfiguring-dependabot-security-updates

<!-- v0.7.0 -->

[gh-actions/setup-node-comp-v1_v2.1.2]: https://github.com/actions/setup-node/compare/v1...v2.1.2
[gh-compare-tag-v0.6.0_v0.7.0]: https://github.com/svengreb/tmpl/compare/v0.6.0...v0.7.0
[gh-husky-comp-v4.2.5_v4.3.0]: https://github.com/typicode/husky/compare/v4.2.5...v4.3.0
[gh-lint-staged-comp-v10.2.11_v10.5.1]: https://github.com/okonet/lint-staged/compare/v10.2.11...v10.5.1
[gh-prettier-comp-2.0.5_2.1.2]: https://github.com/prettier/prettier/compare/2.0.5...2.1.2

<!-- v0.8.0 -->

[gh-compare-tag-v0.7.0_v0.8.0]: https://github.com/svengreb/tmpl/compare/v0.7.0...v0.8.0
[gh-docs-dep_sec_updates]: https://docs.github.com/en/free-pro-team@latest/github/managing-security-vulnerabilities/about-dependabot-security-updates
[gh-docs-dep_updates#schedule_interval]: https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/configuration-options-for-dependency-updates#scheduleinterval
[gh-docs-repo_vis]: https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/setting-repository-visibility
[gh-feat-sec]: https://github.com/security
[gh-prettier/prettier-comp-v2.1.2_v2.2.1]: https://github.com/prettier/prettier/compare/2.1.2...2.2.1
[gh-prettier/prettier]: https://github.com/prettier/prettier
[prettier-blog-rl_2.2]: https://prettier.io/blog/2020/11/20/2.2.0.html

<!-- v0.9.0 -->

[devto-gajus-stop_lockfile-comment-yarn_maintainer]: https://dev.to/arcanis/comment/fo33
[devto-gajus-stop_lockfile]: https://dev.to/gajus/stop-using-package-lock-json-or-yarn-lock-3ddi
[gh-actions/setup-node-comp-v2.1.2_v2.1.5]: https://github.com/actions/setup-node/compare/v2.1.2...v2.1.5
[gh-airbnb/javascript-2409]: https://github.com/airbnb/javascript/issues/2409
[gh-blog-npm_joins]: https://github.blog/2020-03-16-npm-is-joining-github
[gh-compare-tag-v0.8.0_v0.9.0]: https://github.com/svengreb/tmpl/compare/v0.8.0...v0.9.0
[gh-npm/cli-rel-v7.7.0]: https://github.com/npm/cli/releases/tag/v7.7.0
[gh-okonet/lint-staged-comp-v10.5.1_v10.5.4]: https://github.com/typicode/husky/compare/v10.5.1...v10.5.4
[gh-remarkjs/remark-comp-v8.0.1_v9.0.0]: https://github.com/typicode/husky/compare/v8.0.1...v9.0.0
[gh-remarkjs/remark]: https://github.com/remarkjs/remark/releases
[gh-sindresorhus/ama-479#c-310661514]: https://github.com/sindresorhus/ama/issues/479#issuecomment-310661514
[gh-typicode/husky-comp-v4.3.0_v6.0.0]: https://github.com/typicode/husky/compare/v4.3.0...v6.0.0
[gh-yarnpkg/berry]: https://github.com/yarnpkg/berry
[node-dist-v15.13.0]: https://nodejs.org/dist/v15.13.0
[npm-blog-v7_keep_lock]: https://blog.npmjs.org/post/621733939456933888/npm-v7-series-why-keep-package-lockjson.html
[npm-docs-pkgjson#engines]: https://docs.npmjs.com/cli/v7/configuring-npm/package-json#engines
[npm-docs-v7-lockfile]: https://docs.npmjs.com/cli/v7/configuring-npm/package-lock-json
[npm-docs-ws]: https://docs.npmjs.com/cli/v7/using-npm/workspaces
[npm-webapp-semver]: https://semver.npmjs.com
[npm]: https://www.npmjs.com
[snyk-blog-lockfile]: https://snyk.io/blog/why-npm-lockfiles-can-be-a-security-blindspot-for-injecting-malicious-modules
[tw-arcanis-1164229994165559299-comment-19]: https://twitter.com/arcanis/status/1164229994165559299?s=19
[tw-bcrypt-1208950722097598465]: https://twitter.com/bcrypt/status/1208950722097598465
[tw-renovatebot-1163789817492230144]: https://twitter.com/renovatebot/status/1163789817492230144
[wiki-monorepo]: https://en.wikipedia.org/wiki/Monorepo
[yarn-blog-lockfiles]: https://classic.yarnpkg.com/blog/2016/11/24/lockfiles-for-all
[yarn-docs-ws]: https://classic.yarnpkg.com/en/docs/workspaces
[yarn-v1]: https://classic.yarnpkg.com
