# Totara still serves the course index before login

The Totara connector is alpha. Totara is a Moodle-family product. This note uses the Moodle™ 4.5 wording for the same setting name, and it does not claim a Totara-only code path beyond that.

In Moodle 4.5 `lang/en/admin.php`, `forcelogin` is “Force users to log in”. `configforcelogin` says:

> Normally, the site home and the course listings (but not courses) can be read by people without logging in to the site. If you want to force people to log in before they do ANYTHING on the site, then you should enable this setting.

The leftover is that setting left off when campus policy now requires a login before the home page and the course index. The string is explicit that courses themselves are not in the anonymous listing. Do not write that a course body is public. Do not write that a hidden course stays readable. A hidden course is a different rule.

## On a Totara test site you administer

1. Confirm the product is Totara and record the release from the admin footer.
2. Find “Force users to log in” in site administration. Record on or off. The Moodle path is Site administration → Security → Site security settings. Use the label if the menu id differs.
3. From a private browser with no session, request the site home and the course index. You should see those pages when the setting is off, which matches the string.
4. From the same browser, open one enrolled course by its URL. You should be sent to login. If the course body is visible with no session, stop and treat that as a separate visibility bug. It is not what this setting claims.
5. Turn the setting on only when policy says the home page and the course index require a login. The string says people must log in before they do anything on the site. Check the test home page, the course index, and a logged-in learner still opening their course.

Anonymous catalogue pages that you still want public mean the setting stays off, and the row should say that on purpose. An off setting that nobody chose, after a hostname move or a clone, is the leftover.

I publish these notes while working on [GuardLMS](https://guardlms.com/en/integrations/totara). The inventory above is the procedure. It is not a claim that the product runs it for you.
