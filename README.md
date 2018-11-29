# Fun with React and Git Hooks!

This project was created to test adding git hooks to a React project using the `husky` package. My goal was to make it so that, prior to either commiting code or pushing to a git repository, both the `eslint` linter and `jest` test suite must run.

## Installation

If you want to see the final product in action, clone down this repository, run `npm install` to install all the dependencies, make a code change, and commit it. you should see both `eslint` and `jest` run prior to committing the code. If you make a test-breaking change (or one that violates an `eslint` rule), the pre-commit hook will fail and your code will not be committed.

## Set Up from Scratch

Setting this up from scratch was fairly trivial. I started out by boostapping with `create-react-app`

```
create-react-app fun-with-git-hooks
cd fun-with-git-hooks
```

Next, I installed [husky](https://github.com/typicode/husky), which claims to be "git hooks made easy." (Accurate!). Since it's only necessary in the dev environment, only install it as a dev dependency.

```
npm install husky --save-dev
```

We actually end up needing one additional dev dependency called `cross-env`, which will allow us to configure a CI environment variable in whatever environment we're currently in.

```
npm install cross-env --save-dev
```

Finally, let's make some modifications to our `package.json` file to accomplish a few things:

- Reconfigure `jest` tests to be run in Continuous Integration mode
- Add a linting command (we didn't have to install `eslint` seperately as it bootstraps with `create-react-app`)
- Configure our `husky` hooks to first lint and then test

```
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "cross-env CI=true react-scripts test --env=jsdom",
    "eject": "react-scripts eject",
    "lint": "eslint src"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint && npm test",
      "pre-push": "npm run lint && npm test"
    }
  }
```

And that's it! Now, whenever you try to commit or push your code, you will be prevented from doing so if linting or testing fails. Quality for the win!
