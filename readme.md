# lint-staged-django-react-demo

Example repo to demonstrate use of [`lint-staged`][lint-staged] to lint files
outside of project directory.

_This project was scaffolded using
[`django-react-template`][django-react-template]._

## Project structure

The `demo` Django project has 2 appropriately named folders, `backend` and
`frontend`:

```
$ tree -L 3
.
├── demo
│   ├── backend
│   │   ├── apps
│   │   ├── __init__.py
│   │   ├── pagination.py
│   │   ├── permissions.py
│   │   ├── README.md
│   │   ├── requirements
│   │   ├── settings
│   │   ├── static
│   │   ├── templates
│   │   ├── urls.py
│   │   ├── views.py
│   │   └── wsgi.py
│   ├── frontend
│   │   ├── build
│   │   ├── node_modules
│   │   ├── package.json
│   │   ├── README.md
│   │   ├── src
│   │   ├── webpack.config.dev.js
│   │   └── webpack.config.prod.js
│   ├── LICENSE
│   ├── manage.py
│   ├── README.md
│   └── screenshot.png
├── license
├── Pipfile
├── Pipfile.lock
└── readme.md

11 directories, 20 files
```

## Problem

Run linters(ex: [`eslint`][eslint], [`pylint`][pylint]) on the project files,
not restricted to the current package where `lint-staged` is installed. In this
case, `lint-staged` is installed inside the `frontend` package.

## Solution

By default, `lint-staged` filters the staged files to lint only the files which
are inside the Node.js package. Which means that only the files inside
`frontend` would be picked. However, it is possible to lint any files outside of
the package using patterns which start with `../`:

**package.json**:

```json
{
  "...": "...",
  "lint-staged": {
    "linters": {
      "*.js": ["eslint --fix", "git add"],
      "../backend/**/*.py": "pipenv run pylint"
    }
  }
}
```

This configuration runs `eslint`, `pylint` on the relevant files inside the
`frontend` and `backend` folders respectively:

<details>
<summary>Example output</summary>

```
$ git commit
husky > npm run -s precommit (node v10.10.0)

  lint-staged:bin Running `lint-staged@7.3.0` +0ms
  lint-staged:find-bin Loaded package.json using `process.cwd()` +0ms
  lint-staged Loading config using `cosmiconfig` +0ms
  lint-staged Successfully loaded config from `/home/me/dev/oss/lint-staged-django-react-demo/demo/frontend/package.json`:
  lint-staged { linters:
  lint-staged    { '*.js': [ 'eslint --fix', 'git add' ],
  lint-staged      '../backend/**/*.py': 'pipenv run pylint' } } +15ms
  lint-staged:cfg Normalizing config +0ms
  lint-staged:cfg Validating config +2ms
Running lint-staged with the following config:
{
  linters: {
    '*.js': [
      'eslint --fix',
      'git add'
    ],
    '../backend/**/*.py': 'pipenv run pylint'
  },
  concurrent: true,
  chunkSize: 9007199254740991,
  globOptions: {
    matchBase: true,
    dot: true
  },
  ignore: [],
  subTaskConcurrency: 1,
  renderer: 'verbose'
}
  lint-staged:run Running all linter scripts +0ms
  lint-staged:run Resolved git directory to be `/home/me/dev/oss/lint-staged-django-react-demo/` +1ms
  lint-staged:run Loaded list of staged files in git:
  lint-staged:run [ 'readme.md',
  lint-staged:run   'demo/frontend/src/index.js',
  lint-staged:run   'demo/frontend/package.json',
  lint-staged:run   'demo/backend/pagination.py',
  lint-staged:run   'Pipfile.lock',
  lint-staged:run   'Pipfile' ] +19ms
  lint-staged:gen-tasks Generating linter tasks +0ms
  lint-staged:cfg Normalizing config +25ms
  lint-staged:gen-tasks Generated task:
  lint-staged:gen-tasks { pattern: '*.js',
  lint-staged:gen-tasks   commands: [ 'eslint --fix', 'git add' ],
  lint-staged:gen-tasks   fileList:
  lint-staged:gen-tasks    [ '/home/me/dev/oss/lint-staged-django-react-demo/demo/frontend/src/index.js' ] } +11ms
  lint-staged:gen-tasks Generated task:
  lint-staged:gen-tasks { pattern: '../backend/**/*.py',
  lint-staged:gen-tasks   commands: 'pipenv run pylint',
  lint-staged:gen-tasks   fileList:
  lint-staged:gen-tasks    [ '/home/me/dev/oss/lint-staged-django-react-demo/demo/backend/pagination.py' ] } +5ms
Running tasks for *.js [started]
Running tasks for ../backend/**/*.py [started]
  lint-staged:make-cmd-tasks Creating listr tasks for commands [ 'eslint --fix', 'git add' ] +0ms
  lint-staged:find-bin Resolving binary for command `eslint --fix` +156ms
  lint-staged:find-bin Binary for `eslint --fix` resolved to `/home/me/dev/oss/lint-staged-django-react-demo/demo/frontend/node_modules/.bin/eslint` +3ms
  lint-staged:task ✔  OS: linux; File path chunking unnecessary +0ms
  lint-staged:find-bin Resolving binary for command `git add` +0ms
  lint-staged:find-bin Binary for `git add` resolved to `/usr/lib/git-core/git` +1ms
  lint-staged:task ✔  OS: linux; File path chunking unnecessary +1ms
  lint-staged:make-cmd-tasks Creating listr tasks for commands 'pipenv run pylint' +5ms
  lint-staged:find-bin Resolving binary for command `pipenv run pylint` +1ms
  lint-staged:find-bin Binary for `pipenv run pylint` resolved to `/home/me/.local/bin/pipenv` +1ms
  lint-staged:task ✔  OS: linux; File path chunking unnecessary +2ms
eslint --fix [started]
pipenv run pylint [started]
  lint-staged:task bin: /home/me/dev/oss/lint-staged-django-react-demo/demo/frontend/node_modules/.bin/eslint +0ms
  lint-staged:task args: [ '--fix',
  lint-staged:task   '/home/me/dev/oss/lint-staged-django-react-demo/demo/frontend/src/index.js' ] +0ms
  lint-staged:task opts: { reject: false } +0ms
  lint-staged:task bin: /home/me/.local/bin/pipenv +13ms
  lint-staged:task args: [ 'run',
  lint-staged:task   'pylint',
  lint-staged:task   '/home/me/dev/oss/lint-staged-django-react-demo/demo/backend/pagination.py' ] +0ms
  lint-staged:task opts: { reject: false } +0ms
eslint --fix [completed]
git add [started]
  lint-staged:task bin: /usr/lib/git-core/git +611ms
  lint-staged:task args: [ 'add',
  lint-staged:task   '/home/me/dev/oss/lint-staged-django-react-demo/demo/frontend/src/index.js' ] +0ms
  lint-staged:task opts: { reject: false, cwd: '/home/me/dev/oss/lint-staged-django-react-demo/' } +0ms
git add [completed]
Running tasks for *.js [completed]
pipenv run pylint [failed]
→
Running tasks for ../backend/**/*.py [failed]
→
✖ "pipenv run pylint" found some errors. Please fix them and try committing again.
************* Module backend.pagination
/home/me/dev/oss/lint-staged-django-react-demo/demo/backend/pagination.py:1:0: C0111: Missing module docstring (missing-docstring)
/home/me/dev/oss/lint-staged-django-react-demo/demo/backend/pagination.py:7:0: C0111: Missing class docstring (missing-docstring)

------------------------------------------------------------------
Your code has been rated at 8.82/10 (previous run: 8.33/10, +0.49)

husky > pre-commit hook failed (add --no-verify to bypass)
```

</details>

<br>

**Note**: A simpler solution would be to install `lint-staged` in the root of
the project. But that may not always be possible.

[lint-staged]: https://github.com/okonet/lint-staged
[django-react-template]: https://github.com/scottwoodall/django-react-template
[eslint]: https://eslint.org/
[pylint]: https://www.pylint.org/
