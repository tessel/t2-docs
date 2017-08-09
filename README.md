# Tessel 2 Docs

[![Greenkeeper badge](https://badges.greenkeeper.io/tessel/t2-docs.svg)](https://greenkeeper.io/)

[![Code of Conduct](https://img.shields.io/badge/%E2%9D%A4-code%20of%20conduct-blue.svg?style=flat)](https://github.com/tessel/project/blob/master/CONDUCT.md)

This is a GitBook-formatted guide. You may read the processed document at [https://www.gitbook.com/book/tessel/t2-docs/details](https://www.gitbook.com/book/tessel/t2-docs/details). PRs to this repository are automatically published online.

## Developing the Gitbook

### Editing online

GitBooks are editable online! Visit the [GitBook repository for the Tessel 2 docs](https://www.gitbook.com/book/tessel/t2-docs/activity) to suggest changes through a graphical editing format.

### Requirements

The docs are built using a command line tool called [GitBook](https://www.npmjs.com/package/gitbook), which requires:

* [Node Version 4 or higher](https://nodejs.org/en/)
* [NPM Version 2 or higher](https://docs.npmjs.com/getting-started/installing-node)

### Running GitBook locally

* Clone this repo: `https://github.com/tessel/t2-docs.git` \([Learn more here](https://help.github.com/articles/cloning-a-repository/)\)
* Moving into the docs directory: `cd t2-docs`
* Install the project dependecies: `npm install`
* Start the documentation generation: `npm start`

### Changing the contents

SUMMARY.md is autogenerated using the gitbook-summary tool, using instructions from `book.json`. The command to regenerate is `npm run build`, which is automatically run during the `npm start` command.

### Deployment
The `master` branch of this repo is automatically deployed when new changes are committed. Updates are deployed via GitBook ([see build history by commit](https://www.gitbook.com/book/tessel/t2-docs/activity)) to display at [tessel.io/docs](https://tessel.gitbooks.io/t2-docs/content/).
