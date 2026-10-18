# Contributing to sealsbroken

Contributions are welcome: bug reports, code, tests, documentation, translations,
lesson improvements, and accessibility feedback all help.

These guidelines are defaults for the organization's repositories. Start with
the repository's README and any local contribution or development instructions;
those explain its setup, supported platforms, and project-specific workflow.
Please follow our [Code of Conduct](https://github.com/sealsbroken/.github/blob/main/CODE_OF_CONDUCT.md).

## Report a bug

Search existing issues first. If the problem is not already reported, open an
issue in the affected repository with:

- What you expected and what actually happened.
- The smallest steps or example that reproduces the problem.
- The version or commit, platform, and relevant configuration.
- Relevant logs or screenshots with personal information and credentials removed.

For a lesson bug, include the tutor's question, the child's example response,
and the next behavior. A written or synthetic example is usually enough; avoid
posting recordings or identifying information about children.

For suspected security vulnerabilities, follow the repository's security policy
or private reporting option. If neither is available, contact
[lead@onym.app](mailto:lead@onym.app) rather than opening a public report.

## Suggest an improvement

Explain the problem and the behavior you want, with a concrete example. For
larger changes, start an issue or discussion before investing substantial work
so maintainers and contributors can agree on the scope. Small fixes can go
straight to a pull request.

## Make a change

1. Fork the repository or create a working branch if you have access.
2. Follow the repository's setup instructions and use its supported toolchain.
3. Keep the change focused. Avoid unrelated formatting, dependency updates, or
   generated files unless they are needed for the change.
4. Update relevant documentation and examples. Add or adjust meaningful tests
   for changed behavior, especially regressions and error paths.
5. Run the repository's documented formatting, lint, and test checks. State any
   checks you could not run and why. For documentation-only changes, review the
   wording, links, and rendered formatting.
6. Open a pull request describing the problem, resulting behavior, and validation.
   Link related issues and explain any compatibility changes or remaining limits.

Do not include secrets, private data, or unrelated local changes. Preserve
existing license notices and contribute only material you have permission to
share under the repository's contribution and licensing terms. Each repository's
own license applies; this document does not assign one license to the whole org.

## Learning content and translations

For child-facing learning projects, keep activities concrete and instructions
short. A correction should use skills already introduced, preserve earned
progress, and teach the missing step rather than restart the whole lesson.
Screen-free activities should work through speech and physical actions.

Include examples of correct and incorrect responses when changing a lesson.
Check that the implementation, acceptance criteria, and question graphs agree.
For translations, review full sentences in context, including number agreement,
noun cases, and spoken wording; literal word-for-word substitutions are often
insufficient.

## Review and questions

Maintainers may request changes or suggest a smaller scope. Keep review
conversations constructive and address feedback in the same pull request.
Contributions are merged after maintainer review and the applicable checks.

For public project questions, use the relevant repository's issues or discussions.
For private questions, contact [lead@onym.app](mailto:lead@onym.app).
