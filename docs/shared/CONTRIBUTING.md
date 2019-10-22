How to contribute
======================

One of the easiest ways to contribute is to participate in discussions on GitHub issues. You can also contribute by submitting pull requests with code changes.

## General feedback and discussions?
Start a discussion on the [repository issue tracker](https://github.com/dfds/cag/issues).

## Bugs and feature requests?
For non-security related bugs, log a new issue in the appropriate GitHub repository. Here are some of the most common repositories:

* [Roadmap](https://github.com/dfds/roadmap)
* [Composible Architecture Guidelines](https://github.com/dfds/cag)
* [Capability Service](https://github.com/dfds/capability-service)
* [Infrastructure Modules](https://github.com/dfds/infrastructure-modules)

Or browse the full list of repositories in the [DFDS](https://github.com/dfds/) organization.

## Reporting security issues and bugs
Security issues and bugs should be reported privately, via email, to the DFDS security group at it-security@dfds.com. You should receive a response within 24 hours. If for some reason you do not, please follow up via other channels to ensure we received your original message. Further information, including the DFDS PGP key, can be found in the [Security WIKI](TODO - Should probably be controlled by the security center of excellence).

## Other discussions
Our team members also monitor several other discussion channels/forums:

* [DevOps](https://dev.azure.com/dfds)
* [Slack](http://dfds.slack.com)
* [Teams](https://teams.microsoft.com/)

### Identifying the scale
If you would like to contribute to one of our repositories, first identify the scale of what you would like to contribute. If it is small (grammar/spelling or a bug fix) feel free to start working on a fix. If you are submitting a feature or substantial code contribution, please discuss it with the team. You might also read these two blogs posts on contributing code: [Open Source Contribution Etiquette](http://tirania.org/blog/archive/2010/Dec-31.html) by Miguel de Icaza and [Don't "Push" Your Pull Requests](https://www.igvita.com/2011/12/19/dont-push-your-pull-requests/) by Ilya Grigorik. All code submissions will be rigorously reviewed and tested by the DFDS teams, and only those that meet an extremely high bar for both quality and design/roadmap appropriateness will be merged into the source.

### Submitting a pull request
If you don't know what a pull request is read this article: https://help.github.com/articles/using-pull-requests. Make sure the repository can build and all tests pass. Familiarize yourself with the project workflow and coding conventions. The coding, style, and general engineering guidelines can be viewed [here](ENGINEERING.md) page.

### Tests
-  Tests need to be provided for every bug/feature that is completed.
-  Tests only need to be present for issues that need to be verified by QA (for example, not tasks)
-  If there is a scenario that is far too hard to test there does not need to be a test for it.
  - "Too hard" is determined by the team as a whole.

### Feedback
Your pull request will go through extensive checks by the subject matter experts (SME) on our team. Please be patient; Update your pull request according to feedback from DFDS members until it is approved by one of the team members. After that, one of our team members may adjust the branch you merge into based on the expected release schedule.

## Code of conduct
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).  For more information, see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [cag@dfds.com](mailto:cag@dfds.com) with any additional questions or comments.