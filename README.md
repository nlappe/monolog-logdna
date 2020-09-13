# [LogDNA](https://logdna.com/) handler for [Monolog](https://github.com/Seldaek/monolog)

Monolog backend for logdna. This backend use logdna [ingestion api](https://docs.logdna.com/v1.0/reference#api).

## Reason for the Fork
As everyone can see, this fork still uses the underlying curl implementation rather than Http/Guzzle way of Laravel. (which uses curl aswell, but you know...)
I needed a fast, but timeout-configurable solution, so i went for a fork and adding the usual env/config handling.

Maybe i will change the curl impl to a more laravely way - but as time is always tight it might take a while. 

## Install

Install with compose `composer require nlappe/monolog-logdna-laravel`.

## Usage

In yout config->logging.php add this to your 'channels' array:

        'logdna' => [
            'driver' => 'monolog',
            'level' => 'debug',
            'handler' => Nlappe\Monolog\Handler\LogdnaHandler::class,
            'handler_with' => [
                'ingestion_key' => env('LOG_DNA_API_KEY'),
                'hostname' => config('app.name'),
            ],
            'formatter' => 'default',  // ##### does not work without this!
        ],

Then configure it to be used. E.g. add it to the stack channel as an additional log target.

    'channels' => [
        'stack' => [
            'driver' => 'stack',
            'channels' => ['single', 'logdna'],
            'ignore_exceptions' => false,
        ],
        ...
     ]

## Config
To modify the curl timeouts create the follwoing keys in your logging.php config file:
 <br/>
  `return [`<br/>
  `'curl_connect_timeout' => env('CURL_CONNECT_TIMEOUT', 3),`<br/>
  `'curl_request_timeout' => env('CURL_REQUEST_TIMEOUT', 5),`<br/>
 `]`
 
curl_connect_timeout is responsible for the connection timeout. If curl doesnt receive any bytes in the given amount of seconds it will throw an error.
curl_request_timeout is responsible for the request timeout. if the request takes longer than the specified time in seconds, it will abort and throw an error.
  
## License

This project is licensed under LGPL3.0. See `LICENSE` file for details.

## Versions

Version 1.x is php5 compatible version while 2.x is php7.

## Test

To test the project, simply call `make` or `make test`. Everything runs in docker container.

## Clean

To clean your system, call `make clean`. Take note that if you use the same docker images as this project, you might not want to clean. Read the `Makefile` for more information.
