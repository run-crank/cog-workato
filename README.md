# Workato Cog

This is a [Crank][what-is-crank] Cog for Workato, providing
steps and assertions for you to validate the state and behavior of your
Workato instance.

* [Installation](#installation)
* [Usage](#usage)
* [Development and Contributing](#development-and-contributing)

## Installation

Ensure you have the `crank` CLI and `docker` installed and running locally,
then run the following.  You'll be prompted to enter your Workato
credentials once the Cog is successfully installed.

```shell-session
$ crank cog:install automatoninc/workato
```

Note: You can always re-authenticate later.

## Usage

### Authentication
<!-- run `crank cog:readme automatoninc/workato` to update -->
<!-- authenticationDetails -->
You will be asked for the following authentication details on installation. To avoid prompts in a CI/CD context, you can provide the same details as environment variables.

| Field | Install-Time Environment Variable | Description |
| --- | --- | --- |
| **userAgent** | `CRANK_AUTOMATONINC_WORKATO__USERAGENT` | User Agent String |

```shell-session
# Re-authenticate by running this
$ crank cog:auth automatoninc/workato
```
<!-- authenticationDetailsEnd -->

### Steps
Once installed, the following steps will be available for use in any of your
Scenario files.

<!-- run `crank cog:readme automatoninc/workato` to update -->
<!-- stepDetails -->
| Name (ID) | Expression | Expected Data |
| --- | --- | --- |
| **Check a field on a JSON Placeholder user**<br>(`UserFieldEqualsStep`) | `the (?<field>.+) field on JSON Placeholder user (?<email>.+) should (?<operator>be set\|not be set\|be less than\|be greater than\|be one of\|be\|contain\|not be one of\|not be\|not contain) ?(?<expectation>.+)?` | - `email`: User's email address <br><br>- `field`: Field name to check <br><br>- `operator`: Check Logic (be, not be, contain, not contain, be greater than, be less than, be set, not be set, be one of, or not be one of) <br><br>- `expectedValue`: Expected field value |
<!-- stepDetailsEnd -->

## Development and Contributing
Pull requests are welcome. For major changes, please open an issue first to
discuss what you would like to change. Please make sure to add or update tests
as appropriate.

### Setup

1. Install node.js (v12.x+ recommended)
2. Clone this repository.
3. Install dependencies via `npm install`
4. Run `npm start` to validate the Cog works locally (`ctrl+c` to kill it)
5. Run `crank cog:install --source=local --local-start-command="npm start"` to
   register your local instance of this Cog. You may need to append a `--force`
   flag or run `crank cog:uninstall automatoninc/workato` if you've already
   installed the distributed version of this Cog.

### Adding/Modifying Steps
Modify code in `src/steps` and validate your changes by running
`crank cog:step automatoninc/workato` and selecting your step.

To add new steps, create new step classes in `src/steps`. Use existing steps as
a starting point for your new step(s). Note that you will need to run
`crank registry:rebuild` in order for your new steps to be recognized.

Always add tests for your steps in the `test/steps` folder. Use existing tests
as a guide.

### Modifying the API Client or Authentication Details
Modify the ClientWrapper class at `src/client/client-wrapper.ts`.

- If you need to add or modify authentication details, see the
  `expectedAuthFields` static property.
- If you need to expose additional logic from the wrapped API client, add a new
  public method to the wrapper class or mixins, which can then be called in any
  step.
- It's also possible to swap out the wrapped API client completely. You should
  only have to modify code within this class or mixins to achieve that.

Note that you will need to run `crank registry:rebuild` in order for any
changes to authentication fields to be reflected. Afterward, you can
re-authenticate this Cog by running `crank cog:auth automatoninc/workato`

### Tests and Housekeeping
Tests can be found in the `test` directory and run like this: `npm test`.
Ensure your code meets standards by running `npm run lint`.

[what-is-crank]: https://crank.run?utm_medium=readme&utm_source=automatoninc%2Fworkato
