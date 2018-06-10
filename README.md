# codeceptjs-cmdhelper

> Let your CodeceptJS tests run commands in the console/terminal

This is a [Helper](https://codecept.io/helpers/) for [CodeceptJS](https://codecept.io/) that allows you to run commands in the console/terminal. **It is especially useful for preparing the execution environment before/after executing test cases.**

No external dependencies. It uses NodeJS' [spawn](https://nodejs.org/api/child_process.html#child_process_child_process_spawn_command_args_options).

## Install

```bash
npm install --save-dev codeceptjs-cmdhelper
```

## How to configure it

In your `codecept.json`, include **CmdHelper** in the property **helpers** :

```js
  ...
  "helpers": {
    ...
    "CmdHelper": {
      "require": "./node_modules/codeceptjs-cmdhelper"
    }
  },
  ...
```

#### Additional options

- `options` (*object*): parameters that will be given by default to NodeJS' [spawn](https://nodejs.org/api/child_process.html#child_process_child_process_spawn_command_args_options). Default is `{ shell: true }`.
- `showOutput` (*boolean*): whether it will send the execution output to the console. Default is `true`.

```js
  ...
  "helpers": {
    ...
    "CmdHelper": {
      "require": "./node_modules/codeceptjs-cmdhelper",
      "options": {
          "shell": true
      },
      "showOutput": false
    }
  },
  ...
```

## How to use it

The object `I` of your tests and events will have access to new methods. [See the API](#api).


### Example 1

```js
BeforeSuite( async ( I ) => {
    await I.runCommand( 'echo "Hello world!" >foo.txt' );
} );

AfterSuite( async ( I ) => {
    await I.runCommand( 'rm foo.txt' );
    // await I.runCommand( 'del foo.txt' ); // on Windows
} );

// ... your feature ...

// ... your scenarios ...
```

### Example 2

```js
Feature( 'Foo' );

Scenario( 'Bar', async ( I ) => {
    await I.runCommand( 'mkdir foo' );
} );
```

### Note

Make sure to handle errors properly.

```js
    try {
        const code = await I.runCommand( 'mkdir foo' );
        console.log( 0 === code ? 'success' : 'some problem occurred' );
    } catch ( e ) {
        console.warn( e );
    }
```


## API

```js
/**
 * Executes the given command.
 *
 * @param {string} command Command to execute.
 * @param {object} options Same options as in NodeJS' spawn(). Optional. Default is `{ shell: true }`.
 *
 * @returns Promise with the returning execution status code (0 means success)
 */
runCommand( command, options )
```


## License

MIT © [Thiago Delgado Pinto](https://github.com/thiagodp)