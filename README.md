# Git to Changelog CLI

A simple no-frills CLI tool to create a changelog markdown file based on git commits.

Created for projects that I release in my job at Tremor Video DSP, this tool simply creates a CHANGELOG.md from git commits. I tried other npm packages and found that they did not fit my needs for a plug and play, no customization needed, solution.

## Usage

`npm i git-to-changelog -D`

```
{
  "scripts": {
    "changelog": "git-to-changelog"
  }
}
```
```
npm run changelog
```

## Expectations

- Your package.json has a "version" property.
  - Used for latest version detection.
- `optional` Your package.json has a "homepage" property.
  - Used for defining commit links.
  - GitHub, Bitbucket, and GitLab links are supported.
- `optional` Your repo has SemVer tagged releases (e.g. "v1.0.0" or "1.0.0").
  - Use for version grouping.
  - Though optional, there is no point in using this tool if you don't tag versions.

## What Makes This Package Different

### Versioning

If there are commits __after__ your latest git-tagged commit, versioning happens automatically and follows these rules:

- If the package.json version is _later_ than the latest git-tagged version:
  - Use package.json version (with todays date).
- If the package.json version is _equal_ to the latest git-tagged version:
  - Use the word "Latest" (with todays date).
- If the package.json version is _earlier_ than the latest git-tagged version:
  - Throw an error.

If there are no commits after the latest tag -- which should be the case most of the times you run this tool -- the latest git-tagged version will be your first changelog entry. Note that if there are no tagged commits in your repo, this tool will still work.

### Merged Branches

Merged commits are given special treatment. If this tool finds a merge commit with git's default subject naming convention, the subject text is updated to read `Implement '<branch-name>'` and all commits from that branch are indented.

Example:

```
git log --graph --oneline

*   dddb03a (HEAD -> master, origin/master)  Merge branch 'feat/additional-validation' into master
|\
| * 1027df0 Update inline docs
| * f6f8cd1 Update error e041 message
| * 67e294b Add error e040
|/
* 8c6a1cc Add support for admin-only routes
* 4bd8518 Update token hash to include encoded user name
```

Becomes:

```
- Implement 'feat/additional-validation'
  - Update inline docs
  - Update error e041 message
  - Add error e040
- Add support for admin-only routes
- Update token hash to include encoded user name
```

## Sample Output

<div style="border: 2px solid #999; padding: 30px">
  <h1>Changelog</h1>
  <h2>1.1.0 (2018-02-14)</h2>
  <ul>
  <li><a href="https://git.hosturl.com/commit/30d1878">Improve 'failed to connect to server' logic</a></li>
  <li><a href="https://git.hosturl.com/commit/dddb03a">Implement 'feat/additional-validation'</a></li>
    <ul>
      <li><a href="https://git.hosturl.com/commit/1027df0">Update inline docs</a></li>
      <li><a href="https://git.hosturl.com/commit/f6f8cd1">Update error e041 message</a></li>
      <li><a href="https://git.hosturl.com/commit/67e294b">Add error e040</a></li>
    </ul>
  <li><a href="https://git.hosturl.com/commit/8c6a1cc">Add support for admin-only routes</a></li>
  <li><a href="https://git.hosturl.com/commit/4bd8518">Update token hash to include encoded user name</a></li>
  <li><a href="https://git.hosturl.com/commit/674a378">Whitelist routes to allow skipping authentication</a></li>
  </ul>
  <h2>1.0.0 (2018-01-23)</h2>
  <ul>
  <li><a href="https://git.hosturl.com/commit/08d01af">Update docs</a></li>
  <li><a href="https://git.hosturl.com/commit/0919e58">Finalize 'impressions' route</a></li>
  <li><a href="https://git.hosturl.com/commit/a821a1b">Fix issue with 'setToken' promise</a></li>
  <li><a href="https://git.hosturl.com/commit/3e11cfa">Improve README structure</a></li>
  </ul>
</div>

## License

Copyright (c) 2018 Leandro Silva (http://grafluxe.com)

Released under the MIT License.

See LICENSE.md for entire terms.