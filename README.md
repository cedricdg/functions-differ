# functions-differ

A tool to selectively deploy only the Firebase Functions that changed.

`functions-differ` takes a list of Firebase Functions from your repository and returns a list of functions that changed since its last invocation.
This helps you selectively deploy only the functions that changed, thus saving time during re-deployments.

It detects any changes to a function by bundling it into a single minified file, and calculating a hash for it. This works for changes in the function's code, change in its installed dependencies, or any other local imports.

## Usage

- Create a `.differspec.json` file in the directory containing your Firebase Functions:

```shell
my-firebase-project
    ├── firebase.json
    ├── functions
        ├── src
        └── .differspec.json 
```

- Specify your function names and their paths in `.differspec.json`:

```json
{
    "functions": {
        "login": "src/auth/login/function.ts",
        "register": "src/auth/login/function.ts"
    }
}
```

- Run `functions-differ` in the directory containing `.differspec.json`

```shell
cd my-firebase-project/functions
functions-differ
```

- `functions-differ` will then output a list of functions that need to be (re)deployed. This includes any functions that were changed or added since its last invocation.

```shell
> functions-differ
functions:login,functions:register
```

The default output is suitable for passing to `firebase deploy --only` command.

## Options

`functions-differ` supports the following options:

| Name    | Alias | Description                                      | Default                     |
| ------- | ----- | ------------------------------------------------ | --------------------------- |
| dir     | d     | The directory containing `.differspec.json` file | (current working directory) |
| write   | w     | Write output to `.differspec.json` file or not   | true                        |
| verbose | v     | Output verbose logs                              | false                       |
| prefix  |       | Prefix for each function name output             | `functions:`                |
| sep     |       | Separator for each output function               | `,`                         |

## .differspec.json

`.differspec.json` serves as persistent storage for `functions-differ`. It contains the following properties:

| Name        | Description                                                                                                                   |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `functions` | JSON object containing names of functions in the repository along with their paths                                            |
| `hashes`    | (**AUTOGENERATED, DO NOT MODIFY**) JSON object containing hashes of all functions in the `functions` property                 |
| `lastDiff`  | (**AUTOGENERATED, DO NOT MODIFY**) JSON object containing the results of the last successful invocation of `functions-differ` |

## Contributions

Please discuss bugs, feature requests, and help in Github Issues. Pull requests are welcome, but please make sure to open an issue for your changes first.

## Installation 

*TODO*