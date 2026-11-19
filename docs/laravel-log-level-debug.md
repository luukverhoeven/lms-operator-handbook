# Laravel LOG_LEVEL is still debug on the LMS

A copied Laravel 12 environment logs at `debug` until somebody changes `LOG_LEVEL`. That is a different leftover from `APP_DEBUG`. Debug mode can be off while the log channel is still on `debug`, and the log file still receives the debug stream.

The 12.x `.env.example` sets `LOG_CHANNEL=stack`, `LOG_STACK=single`, and `LOG_LEVEL=debug`. `config/logging.php` points the `single` channel at `storage/logs/laravel.log` with `'level' => env('LOG_LEVEL', 'debug')`. The `daily`, `stderr`, `syslog`, `errorlog`, and `papertrail` channels use the same pattern. The stack channel itself has no level. It forwards to whatever `LOG_STACK` names, and the example names `single`.

The `slack` channel uses `env('LOG_LEVEL', 'critical')`. The fallback `critical` applies only when `LOG_LEVEL` is absent. The example defines `LOG_LEVEL=debug`, so a slack webhook configured later inherits `debug` too.

`APP_DEBUG` is the error-page switch. Leave it for its own check. Do not rotate `APP_KEY` while you change a log level.

## On an app you run

```bash
php artisan tinker --execute="echo config('logging.default'), PHP_EOL, config('logging.channels.single.level'), PHP_EOL;"
```

When the default is `stack` and `LOG_STACK` is `single`, the second line is the level that filters `storage/logs/laravel.log`. `debug` is the leftover on a site learners use.

Tail the log while you load one authenticated page as a test user. Confirm lines are being written, then set `LOG_LEVEL=info` or `warning` in the environment that PHP-FPM or the queue worker actually reads. Reload PHP and the workers. Print `config('logging.channels.single.level')` again.

If slack is configured, print `config('logging.channels.slack.level')` too. Raising `LOG_LEVEL` moves that channel with it, because both read the same variable.

Do not paste log lines that contain session ids, mail addresses, or SQL bindings into a ticket. The row is the level, the channel, and the file path.

I publish these notes while working on [GuardLMS](https://guardlms.com/en/integrations/laravel). The inventory above is the procedure. It is not a claim that the product runs it for you.
