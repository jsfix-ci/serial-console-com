# serial-console-com [![Build Status](https://www.travis-ci.org/eove/serial-console-com.svg?branch=master)](https://www.travis-ci.org/eove/serial-console-com) [![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)

Node.js lib to communicate a unix-like console through a serial line

## Install

`npm install`

## Usage

This lib exposes a communicator which may execute commands through the serial line.

```js
import { createSerialCommunicator } from '@eove/serial-console-com';

const communicator = createSerialCommunicator({
  baudrate: 115200,
  prompt: '/ #',
  lineSeparator: '\n'
});

communicator
  .connect('/dev/ttyUSB0')
  .then(() => communicator.executeCommand('ls -al'))
  .then(({ output, errorCode }) => {
    console.log('error code:', errorCode);
    console.log('output', output.join('\n'));
  });
```

The async `executeCommand` method returns an object with the following fields:

- `errorCode`: a number corresponding to the error code of the command
- `output`: a string array corresponding to the lines of the command output

🔥You can try it from the command line: `npx @eove/serial-console-com run 'ls -al /' -p /dev/ttyUSB0`

Note: the `npx` command exits with the given command error code:

```bash
npx @eove/serial-console-com run 'true' -p /dev/ttyUSB0
echo $?
0
```

```bash
npx @eove/serial-console-com run 'false' -p /dev/ttyUSB0
echo $?
1
```
