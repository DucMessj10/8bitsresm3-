# For most projects, this workflow file will not need changing; you simply need
# to commit it to your repository.
#
# You may wish to alter this file to override the set of languages analyzed,
# or to provide custom queries or build logic.
#
# ******** NOTE ********
# We have attempted to detect the languages in your repository. Please check
# the `language` matrix defined below to confirm you have the correct set of
# supported CodeQL languages.
#
name: "CodeQL"

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  schedule:
    - cron: '19 1 * * 0'

jobs:
  analyze:
    name: Analyze (${{ matrix.language }})
    # Runner size impacts CodeQL analysis time. To learn more, please see:
    #   - https://gh.io/recommended-hardware-resources-for-running-codeql
    #   - https://gh.io/supported-runners-and-hardware-resources
    #   - https://gh.io/using-larger-runners (GitHub.com only)
    # Consider using larger runners or machines with greater resources for possible analysis time improvements.
    runs-on: ${{ (matrix.language == 'swift' && 'macos-latest') || 'ubuntu-latest' }}
    timeout-minutes: ${{ (matrix.language == 'swift' && 120) || 360 }}
    permissions:
      # required for all workflows
      security-events: write

      # required to fetch internal or private CodeQL packs
      packages: read

      # only required for workflows in private repositories
      actions: read
      contents: read

    strategy:
      fail-fast: false
      matrix:
        include:
        # CodeQL supports the following values keywords for 'language': 'c-cpp', 'csharp', 'go', 'java-kotlin', 'javascript-typescript', 'python', 'ruby', 'swift'
        # Use `c-cpp` to analyze code written in C, C++ or both
        # Use 'java-kotlin' to analyze code written in Java, Kotlin or both
        # Use 'javascript-typescript' to analyze code written in JavaScript, TypeScript or both
        # To learn more about changing the languages that are analyzed or customizing the build mode for your analysis,
        # see https://docs.github.com/en/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/customizing-your-advanced-setup-for-code-scanning.
        # If you are analyzing a compiled language, you can modify the 'build-mode' for that language to customize how
        # your codebase is analyzed, see https://docs.github.com/en/code-security/code-scanning/creating-an-advanced-setup-for-code-scanning/codeql-code-scanning-for-compiled-languages
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    # Initializes the CodeQL tools for scanning.
    - name: Initialize CodeQL
      uses: github/codeql-action/init@v3
      with:
        languages: ${{ matrix.language }}
        build-mode: ${{ matrix.build-mode }}
        # If you wish to specify custom queries, you can do so here or in a config file.
        # By default, queries listed here will override any specified in a config file.
        # Prefix the list here with "+" to use these queries and those in the config file.

        # For more details on CodeQL's query packs, refer to: https://docs.github.com/en/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/configuring-code-scanning#using-queries-in-ql-packs
        # queries: security-extended,security-and-quality

    # If the analyze step fails for one of the languages you are analyzing with
    # "We were unable to automatically build your code", modify the matrix above
    # to set the build mode to "manual" for that language. Then modify this step
    # to build your code.
    # ℹ️ Command-line programs to run using the OS shell.
    # 📚 See https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsrun
    - if: matrix.build-mode == 'manual'
      shell: bash
      run: |
        echo 'If you are using a "manual" build mode for one or more of the' \
          'languages you are analyzing, replace this with the commands to build' \
          'your code, for example:'
        echo '  make bootstrap'
        echo '  make release'
        exit 1

    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v3
      with:
        category: "/language:${{matrix.language}}"
                    - name: Setup Node.js environment
  uses: actions/setup-node@v4.0.2
  with:
    # Set always-auth in npmrc.
    always-auth: # optional, default is false
    # Version Spec of the version to use. Examples: 12.x, 10.15.1, >=10.15.0.
    node-version: # optional
    # File containing the version Spec of the version to use.  Examples: package.json, .nvmrc, .node-version, .tool-versions.
    node-version-file: # optional
    # Target architecture for Node to use. Examples: x86, x64. Will use system architecture by default.
    architecture: # optional
    # Set this option if you want the action to check for the latest available version that satisfies the version spec.
    check-latest: # optional
    # Optional registry to set up for auth. Will set the registry in a project level .npmrc and .yarnrc file, and set up auth to read in from env.NODE_AUTH_TOKEN.
    registry-url: # optional
    # Optional scope for authenticating against scoped registries. Will fall back to the repository owner when using the GitHub Packages registry (https://npm.pkg.github.com/).
    scope: # optional
    # Used to pull node distributions from node-versions. Since there's a default, this is typically not supplied by the user. When running this action on github.com, the default value is sufficient. When running on GHES, you can pass a personal access token for github.com if you are experiencing rate limiting.
    token: # optional, default is ${{ github.server_url == 'https://github.com' && github.token || '' }}
    # Used to specify a package manager for caching in the default directory. Supported values: npm, yarn, pnpm.
    cache: # optional
    # Used to specify the path to a dependency file: package-lock.json, yarn.lock, etc. Supports wildcards or a list of file names for caching multiple dependencies.
    cache-dependency-path: # optional
                - name: Setup Go environment
  uses: actions/setup-go@v5.0.1
  with:
    # The Go version to download (if necessary) and use. Supports semver spec and ranges. Be sure to enclose this option in single quotation marks.
    go-version: # optional
    # Path to the go.mod or go.work file.
    go-version-file: # optional
    # Set this option to true if you want the action to always check for the latest available version that satisfies the version spec
    check-latest: # optional
    # Used to pull Go distributions from go-versions. Since there's a default, this is typically not supplied by the user. When running this action on github.com, the default value is sufficient. When running on GHES, you can pass a personal access token for github.com if you are experiencing rate limiting.
    token: # optional, default is ${{ github.server_url == 'https://github.com' && github.token || '' }}
    # Used to specify whether caching is needed. Set to true, if you'd like to enable caching.
    cache: # optional, default is true
    # Used to specify the path to a dependency file - go.sum
    cache-dependency-path: # optional
    # Target architecture for Go to use. Examples: x86, x64. Will use system architecture by default.
    architecture: # optional
                  - name: Close Stale Issues
  uses: actions/stale@v9.0.0
  with:
    # Token for the repository. Can be passed in using `{{ secrets.GITHUB_TOKEN }}`.
    repo-token: # optional, default is ${{ github.token }}
    # The message to post on the issue when tagging it. If none provided, will not mark issues stale.
    stale-issue-message: # optional
    # The message to post on the pull request when tagging it. If none provided, will not mark pull requests stale.
    stale-pr-message: # optional
    # The message to post on the issue when closing it. If none provided, will not comment when closing an issue.
    close-issue-message: # optional
    # The message to post on the pull request when closing it. If none provided, will not comment when closing a pull requests.
    close-pr-message: # optional
    # The number of days old an issue or a pull request can be before marking it stale. Set to -1 to never mark issues or pull requests as stale automatically.
    days-before-stale: # optional, default is 60
    # The number of days old an issue can be before marking it stale. Set to -1 to never mark issues as stale automatically. Override "days-before-stale" option regarding only the issues.
    days-before-issue-stale: # optional
    # The number of days old a pull request can be before marking it stale. Set to -1 to never mark pull requests as stale automatically. Override "days-before-stale" option regarding only the pull requests.
    days-before-pr-stale: # optional
    # The number of days to wait to close an issue or a pull request after it being marked stale. Set to -1 to never close stale issues or pull requests.
    days-before-close: # optional, default is 7
    # The number of days to wait to close an issue after it being marked stale. Set to -1 to never close stale issues. Override "days-before-close" option regarding only the issues.
    days-before-issue-close: # optional
    # The number of days to wait to close a pull request after it being marked stale. Set to -1 to never close stale pull requests. Override "days-before-close" option regarding only the pull requests.
    days-before-pr-close: # optional
    # The label to apply when an issue is stale.
    stale-issue-label: # optional, default is Stale
    # The label to apply when an issue is closed.
    close-issue-label: # optional
    # The labels that mean an issue is exempt from being marked stale. Separate multiple labels with commas (eg. "label1,label2").
    exempt-issue-labels: # optional, default is 
    # The reason to use when closing an issue.
    close-issue-reason: # optional, default is not_planned
    # The label to apply when a pull request is stale.
    stale-pr-label: # optional, default is Stale
    # The label to apply when a pull request is closed.
    close-pr-label: # optional
    # The labels that mean a pull request is exempt from being marked as stale. Separate multiple labels with commas (eg. "label1,label2").
    exempt-pr-labels: # optional, default is 
    # The milestones that mean an issue or a pull request is exempt from being marked as stale. Separate multiple milestones with commas (eg. "milestone1,milestone2").
    exempt-milestones: # optional, default is 
    # The milestones that mean an issue is exempt from being marked as stale. Separate multiple milestones with commas (eg. "milestone1,milestone2"). Override "exempt-milestones" option regarding only the issues.
    exempt-issue-milestones: # optional, default is 
    # The milestones that mean a pull request is exempt from being marked as stale. Separate multiple milestones with commas (eg. "milestone1,milestone2"). Override "exempt-milestones" option regarding only the pull requests.
    exempt-pr-milestones: # optional, default is 
    # Exempt all issues and pull requests with milestones from being marked as stale. Default to false.
    exempt-all-milestones: # optional, default is false
    # Exempt all issues with milestones from being marked as stale. Override "exempt-all-milestones" option regarding only the issues.
    exempt-all-issue-milestones: # optional, default is 
    # Exempt all pull requests with milestones from being marked as stale. Override "exempt-all-milestones" option regarding only the pull requests.
    exempt-all-pr-milestones: # optional, default is 
    # Only issues or pull requests with all of these labels are checked if stale. Defaults to `` (disabled) and can be a comma-separated list of labels.
    only-labels: # optional, default is 
    # Only issues or pull requests with at least one of these labels are checked if stale. Defaults to `` (disabled) and can be a comma-separated list of labels.
    any-of-labels: # optional, default is 
    # Only issues with at least one of these labels are checked if stale. Defaults to `` (disabled) and can be a comma-separated list of labels. Override "any-of-labels" option regarding only the issues.
    any-of-issue-labels: # optional, default is 
    # Only pull requests with at least one of these labels are checked if stale. Defaults to `` (disabled) and can be a comma-separated list of labels. Override "any-of-labels" option regarding only the pull requests.
    any-of-pr-labels: # optional, default is 
    # Only issues with all of these labels are checked if stale. Defaults to `[]` (disabled) and can be a comma-separated list of labels. Override "only-labels" option regarding only the issues.
    only-issue-labels: # optional, default is 
    # Only pull requests with all of these labels are checked if stale. Defaults to `[]` (disabled) and can be a comma-separated list of labels. Override "only-labels" option regarding only the pull requests.
    only-pr-labels: # optional, default is 
    # The maximum number of operations per run, used to control rate limiting (GitHub API CRUD related).
    operations-per-run: # optional, default is 30
    # Remove stale labels from issues and pull requests when they are updated or commented on.
    remove-stale-when-updated: # optional, default is true
    # Remove stale labels from issues when they are updated or commented on. Override "remove-stale-when-updated" option regarding only the issues.
    remove-issue-stale-when-updated: # optional, default is 
    # Remove stale labels from pull requests when they are updated or commented on. Override "remove-stale-when-updated" option regarding only the pull requests.
    remove-pr-stale-when-updated: # optional, default is 
    # Run the processor in debug mode without actually performing any operations on live issues.
    debug-only: # optional, default is false
    # The order to get issues or pull requests. Defaults to false, which is descending.
    ascending: # optional, default is false
    # Delete the git branch after closing a stale pull request.
    delete-branch: # optional, default is false
    # The date used to skip the stale action on issue/pull request created before it (ISO 8601 or RFC 2822).
    start-date: # optional, default is 
    # The assignees which exempt an issue or a pull request from being marked as stale. Separate multiple assignees with commas (eg. "user1,user2").
    exempt-assignees: # optional, default is 
    # The assignees which exempt an issue from being marked as stale. Separate multiple assignees with commas (eg. "user1,user2"). Override "exempt-assignees" option regarding only the issues.
    exempt-issue-assignees: # optional, default is 
    # The assignees which exempt a pull request from being marked as stale. Separate multiple assignees with commas (eg. "user1,user2"). Override "exempt-assignees" option regarding only the pull requests.
    exempt-pr-assignees: # optional, default is 
    # Exempt all issues and pull requests with assignees from being marked as stale. Default to false.
    exempt-all-assignees: # optional, default is false
    # Exempt all issues with assignees from being marked as stale. Override "exempt-all-assignees" option regarding only the issues.
    exempt-all-issue-assignees: # optional, default is 
    # Exempt all pull requests with assignees from being marked as stale. Override "exempt-all-assignees" option regarding only the pull requests.
    exempt-all-pr-assignees: # optional, default is 
    # Exempt draft pull requests from being marked as stale. Default to false.
    exempt-draft-pr: # optional, default is false
    # Display some statistics at the end regarding the stale workflow (only when the logs are enabled).
    enable-statistics: # optional, default is true
    # A comma delimited list of labels to add when an issue or pull request becomes unstale.
    labels-to-add-when-unstale: # optional, default is 
    # A comma delimited list of labels to remove when an issue or pull request becomes stale.
    labels-to-remove-when-stale: # optional, default is 
    # A comma delimited list of labels to remove when an issue or pull request becomes unstale.
    labels-to-remove-when-unstale: # optional, default is 
    # Any update (update/comment) can reset the stale idle time on the issues and pull requests.
    ignore-updates: # optional, default is false
    # Any update (update/comment) can reset the stale idle time on the issues. Override "ignore-updates" option regarding only the issues.
    ignore-issue-updates: # optional, default is 
    # Any update (update/comment) can reset the stale idle time on the pull requests. Override "ignore-updates" option regarding only the pull requests.
    ignore-pr-updates: # optional, default is 
    # Only the issues or the pull requests with an assignee will be marked as stale automatically.
    include-only-assigned: # optional, default is false
                - name: Upload a Build Artifact
  uses: actions/upload-artifact@v4.3.3
  with:
    # Artifact name
    name: # optional, default is artifact
    # A file, directory or wildcard pattern that describes what to upload
    path: 
    # The desired behavior if no files are found using the provided path.
Available Options:
  warn: Output a warning but do not fail the action
  error: Fail the action with an error message
  ignore: Do not output any warnings or errors, the action does not fail

    if-no-files-found: # optional, default is warn
    # Duration after which artifact will expire in days. 0 means using default retention.
Minimum 1 day. Maximum 90 days unless changed from the repository settings page.

    retention-days: # optional
    # The level of compression for Zlib to be applied to the artifact archive. The value can range from 0 to 9: - 0: No compression - 1: Best speed - 6: Default compression (same as GNU Gzip) - 9: Best compression Higher levels will result in better compression, but will take longer to complete. For large files that are not easily compressed, a value of 0 is recommended for significantly faster uploads.

    compression-level: # optional, default is 6
    # If true, an artifact with a matching name will be deleted before a new one is uploaded. If false, the action will fail if an artifact for the given name already exists. Does not fail if the artifact does not exist.

    overwrite: # optional, default is false
                - name: Upload a Build Artifact
  uses: actions/upload-artifact@v4.3.3
  with:
    # Artifact name
    name: # optional, default is artifact
    # A file, directory or wildcard pattern that describes what to upload
    path: 
    # The desired behavior if no files are found using the provided path.
Available Options:
  warn: Output a warning but do not fail the action
  error: Fail the action with an error message
  ignore: Do not output any warnings or errors, the action does not fail

    if-no-files-found: # optional, default is warn
    # Duration after which artifact will expire in days. 0 means using default retention.
Minimum 1 day. Maximum 90 days unless changed from the repository settings page.

    retention-days: # optional
    # The level of compression for Zlib to be applied to the artifact archive. The value can range from 0 to 9: - 0: No compression - 1: Best speed - 6: Default compression (same as GNU Gzip) - 9: Best compression Higher levels will result in better compression, but will take longer to complete. For large files that are not easily compressed, a value of 0 is recommended for significantly faster uploads.

    compression-level: # optional, default is 6
    # If true, an artifact with a matching name will be deleted before a new one is uploaded. If false, the action will fail if an artifact for the given name already exists. Does not fail if the artifact does not exist.

    overwrite: # optional, default is false
                - name: Setup .NET Core SDK
  uses: actions/setup-dotnet@v4.0.0
  with:
    # Optional SDK version(s) to use. If not provided, will install global.json version when available. Examples: 2.2.104, 3.1, 3.1.x, 3.x, 6.0.2xx
    dotnet-version: # optional
    # Optional quality of the build. The possible values are: daily, signed, validated, preview, ga.
    dotnet-quality: # optional
    # Optional global.json location, if your global.json isn't located in the root of the repo.
    global-json-file: # optional
    # Optional package source for which to set up authentication. Will consult any existing NuGet.config in the root of the repo and provide a temporary NuGet.config using the NUGET_AUTH_TOKEN environment variable as a ClearTextPassword
    source-url: # optional
    # Optional OWNER for using packages from GitHub Package Registry organizations/users other than the current repository's owner. Only used if a GPR URL is also provided in source-url
    owner: # optional
    # Optional NuGet.config location, if your NuGet.config isn't located in the root of the repo.
    config-file: # optional
    # Optional input to enable caching of the NuGet global-packages folder
    cache: # optional
    # Used to specify the path to a dependency file: packages.lock.json. Supports wildcards or a list of file names for caching multiple dependencies.
    cache-dependency-path: # optional
                - name: First interaction
  uses: actions/first-interaction@v1.3.0
  with:
    # Token for the repository. Can be passed in using {{ secrets.GITHUB_TOKEN }}
    repo-token: 
    # Comment to post on an individual's first issue
    issue-message: # optional
    # Comment to post on an individual's first pull request
    pr-message: # optional
                      
          
