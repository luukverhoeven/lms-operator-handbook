# WordPress still has Anyone can register on the LMS

An LMS on WordPress that still offers public registration is the leftover, when enrolment was supposed to move to cohorts, SSO, or a paid gateway. The option is `users_can_register`.

WordPress 6.7 `wp-admin/options-general.php` renders it as a checkbox named `users_can_register`, value `1`, checked when `get_option('users_can_register')` is `1`. The label is “Anyone can register”.

That checkbox does not create Application Passwords, and it does not speak `xmlrpc.php`. Those are other doors. Closing this one leaves existing accounts in place. It stops new self-registrations.

## On a site you administer

```bash
wp option get users_can_register
```

`1` means the checkbox is on. `0` or an empty result means this leftover is already closed. Record the value before you change it.

In a private browser, request the register action and do not submit it:

```text
GET /wp-login.php?action=register
```

Read the response. A registration form means the option is on. A notice that registration is disabled means it is off. Do not create an account. Do not type a password into the form.

If self-registration is not the enrolment path, uncheck “Anyone can register” under Settings → General and save. Run `wp option get users_can_register` again and expect `0`. Reload the register action logged out and expect the disabled notice.

Write one row: option value, whether the form was shown, the enrolment path that replaced it (cohort, SSO, manual), and the date. Existing users stay. Password resets for those users stay. This option does not disable them.

I publish these notes while working on [GuardLMS](https://guardlms.com/en/integrations/wordpress). The inventory above is the procedure. It is not a claim that the product runs it for you.
