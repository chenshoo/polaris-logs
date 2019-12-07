![Polaris-logo](static/img/polaris-logo.png)
# polaris-logs
[![Build Status](https://travis-ci.com/Enigmatis/polaris-logs.svg?branch=develop)](https://travis-ci.com/Enigmatis/polaris-logs)
[![NPM version](https://img.shields.io/npm/v/@enigmatis/polaris-logs.svg?style=flat-square)](https://www.npmjs.com/package/@enigmatis/polaris-logs)

Write your logs easily in a standardized manner!

Ever wanted your logs to look pretty, to contain all the data you need in order to monitor your system, and to be written in an extremely easy way? You can now do this, using this library.

### LoggerConfiguration
Through this interface you should set the following configuration to the ``PolarisLogger``:

+ **loggerLevel** (*string*) - The level the logger is listening on, can be one of the following levels: ``fatal`` / 
``error`` / ``warn`` / ``info`` / ``trace`` / ``debug``.
+ **logstashConfigurations** (*LogstashConfiguration[] - optional*) - Through this property you can set multiple logstash
hosts and ports (**Notice that we use UDP to write logs to the logstash services**).
+ **writeToConsole** (*boolean - optional*) - Determines if the logger should write the logs to the console.
+ **writeFullMessageToConsole** (*boolean - optional*) - Set this property to ``true``, if you decide to write full 
detailed logs to the console, since only the ``timestamp`` accompanied by the ``log level``, ``message`` and 
``throwable`` will be written by default.
+ **logFilePath** (*string - optional*) - If provided, the logs will be written to the specified path.
+ **dailyRotateFileConfiguration** (*DailyRotateFileConfiguration - optional*) - If you are interested in daily log file
instead of just **one** log file, see the configuration section below. It creates a log file for each day. Those daily
log files deleted after ``X`` days after being created. **If provided, it ignores the logFilePath property.**
+ **customTransports** (*Transport[] - optional*) - Array of custom transports you can provide to the winston logger.

### DailyRotateFileConfiguration
+ **directoryPath** (*string*) - The directory path, where the daily files will be located.
+ **fileNamePrefix** (*string*) - The current date in the format of ``DD-MM-YYYY`` will be added to the name prefix.
+ **fileExtension** (*string*) - The extension of the log file (without the dot).
+ **numberOfDaysToDeleteFile** (*number - optional*) - Number of days till old log files will be deleted, default is 30
days.

### ApplicationProperties
This interface represent the application configurable log properties.

Those properties are:
 + id
 + name
 + version
 + environment
 + component

### PolarisLogProperties
This interface represent the log properties that will be logged through the ``PolarisLogger``.

### PolarisLogger
This class interacts with the actual winston logger and responsible for logging the properties that was provided to him.

### Example

```TypeScript

import { ApplicationProperties, LoggerConfiguration, PolarisLogger } from '@enigmatis/polaris-logs';

const appProps: ApplicationProperties = {
    id: 'p0laris-l0gs',
    name: 'polaris-logs',
    version: 'v1',
    environment: 'environment',
    component: 'component',
};

const logstashConf = [{
    logstashHost: '127.0.0.1',
    logstashPort: 3000,
}, {
    logstashHost: '8.8.8.8',
    logstashPort: 6000,
}];

const logConf: LoggerConfiguration = {
    loggerLevel: 'trace',
    logstashConfigurations: logstashConf,
    writeToConsole: true,
    writeFullMessageToConsole: true,
    // logFilePath: 'D:\\example.txt',
    dailyRotateFileConfiguration: {
        directoryPath: 'D:\\',
        fileNamePrefix: 'polaris',
        fileExtension: 'txt',
        numberOfDaysToDeleteFile: 60,
    },
};

const logger = new PolarisLogger(logConf, appProps);

logger.fatal('fatal message', { elapsedTime: 500, eventKind: 'foo' });
logger.error('error message', { elapsedTime: 15000, throwable: new Error('oops') });
logger.warn('warn message');
logger.info('info message');
logger.debug('debug message');
logger.trace('trace message');

```

For any additional help and requests, feel free to contact us :smile:
