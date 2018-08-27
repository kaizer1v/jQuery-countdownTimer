# File structure

- `dist/` has output files. It is re-created via  `npm run build`. It has:
  - [g1.js](dist/g1.js) - non-minified source created via rollup on [index.js](index.js)
  - [g1.min.js](dist/g1.min.js) - minified source created via rollup on [index.js](index.js)
  - `<module>.min.js` for each module - minified source created via rollup on `index-<module>.js`
- `./` has setup files.
  - [index.js](index.js) is the rollup configuration to create the full `g1` package
  - `index-<module>.js` is the rollup configuration to create each module
  - Other support files
- `src/` has source files. This includes:
  - `<module>.js` - underlying source for each module, which may import other dependencies. TODO: rename jquery.* to this
  - `<library>.js` - for internally used libraries
- `test/` has test cases. It is run via `npm test`. It has:
  - `test-<module>.html` for each browser module
  - `test-<library>.js` for each library that can be tested directly on node.js
  - `server.js` runs tests on [Puppeteer](https://github.com/GoogleChrome/puppeteer)
  - `tape.js` is dynamically created using browserify to help with test cases. This is not committed
  - Other test dependencies

# Set up

To set up locally, clone this repo and run:

    yarn install
    npm run build     # Optional: build the dist/ directory

Here is a list of npm commands available:

    npm test          # Run all unit tests
    npm run lint      # Check for basic errors using eslint
    npm run server    # Start a HTTP server at the current folder for manual testing
    npm run dev       # Start a rollup watch that re-builds dist/ if files change

# Release

To publish a new version on npm:

    # Run tests on dev branch
    git checkout dev
    npm run lint
    npm test

    # Update package.json version
    # Update CHANGELOG.md
    # Ensure that there are no build errors on the server
    git commit . -m"DOC: Release version x.x.x"
    git push

    # Merge into dev branch
    git checkout master
    git merge dev
    git tag -a v0.x.x           # Add a one-line summary
    git push --follow-tags
    npm publish

    git checkout dev
