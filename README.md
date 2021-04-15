
# ci-license-checker

Simple tool to check licenses of all npm dependencies in a project against your approved set of licenses. Can be added to a test suite / CI to get a warning about packages not meeting predefined license requirements. This is a wrapper around [`davglass/license-checker`](https://github.com/davglass/license-checker).

## Acknowledgement

This project is based on the great work of [`Brightspace/d2l-license-checker`](https://github.com/Brightspace/d2l-license-checker).

The aim of this work is to make the solution more generic and customizable at a project level without default config and without a predefined set of licenses.

## How to use

1. Add this package as a development dependency:

    `npm install --save-dev https://github.com/mathieugerard/ci-license-checker`

1. Define a new script in your `package.json` by adding the following lines:

    ```json
    "scripts": {
      "license-check": "ci-license-checker"
    }
    ```

1. Add a config file `.licensechecker.json` to your node module.

    ```json
    {
      "acceptedLicenses": [
        "0BSD",
        "Apache-1.0",
        "Apache-2.0",
        "BSD-2-Clause",
        "BSD-3-Clause",
        "CC-BY-1.0",
        "CC-BY-2.0",
        "CC-BY-2.5",
        "CC-BY-3.0",
        "CC-BY-4.0",
        "CC0-1.0",
        "EPL-1.0",
        "ISC",
        "MIT",
        "Public-Domain",
        "Python-2.0",
        "Unlicense",
        "W3C",
        "WTFPL",
        "X11",
        "Zlib"
      ],
      "acceptedScopes": ["yourCompanyScopeWithoutThe@"],
      "manualOverrides": {
        "some-package@9.9.9": "MIT",
        "some-other-package@*": "ALLOWED (license-name)"
      }
    }
    ```

1. Check that the licenses pass the test by running `npm run license-check`. See `--help` for more options.

1. Make sure `npm run license-check` is called in your CI build script or as part as your tests

If licenses do not pass the test, you can run `npm run license-check -- --generate-template > .licensechecker.template.json` to generate a template file that can be copied and pasted into the config file for easy overrides.

## Configuration file

The configuration file is a simple JSON file with the following optional entries:

* `"acceptedLicenses"`: The list of licenses that are allowed for your project. Values should be valid SPDX ID.

* `"acceptedScopes"`: List of [NPM scopes](https://docs.npmjs.com/misc/scope) that should always be allowed. This is convenient if your team uses its own scoped registry. Do not include the `@` or `/` characters.

* `"manualOverrides"`: Object where each key is a package name and version (see above example), and the value is a valid SPDX ID. The version number can be a semver expression. You can use this to manually specify the license of a package for which the license is not specified in its `package.json` file or where an invalid SPDX ID is used.

  In addition to the [SPDX IDs](https://spdx.org/licenses/), you can use the following strings:

  * `Public-Domain`: identifier for public domain code (not supported by SPDX)
  * `Project-Owner`: identifier indicating that you own this package and that its license can be ignored (doesn't need to be added to `"acceptedlicenses"`)
  * `ALLOWED (license-name)`: identifier indicating that although `license-name` is not in the `"acceptedLicenses"` set, its use has been granted a special permission for this project.

* `"ignoreUnusedManualOverrides"`: Set it to true if you do not want warnings logged when you have unused manual overrides (`false` by default)

## Contributing

1. Update code.
1. Update version in `package.json`.
1. Commit/merge changes via pull request.
