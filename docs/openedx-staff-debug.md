# Open edX still shows Staff Debug to course staff

The Open edX connector this note sits next to is alpha. The setting below is upstream LMS behaviour, read from `edx-platform`, not a claim about that connector.

`lms/envs/common.py` on `openedx/edx-platform` master sets `DISPLAY_DEBUG_INFO_TO_STAFF = True`. The toggle annotation says the default is True, and the description is: add a “Staff Debug” button to course blocks for debugging by course staff. The implementation is a Django setting. Creation date on the annotation is 2015-09-04.

The button is for course staff. It is not a scanner, and it does not list vulnerabilities. Leave it on only when staff still use that panel.

Two neighbours are easy to mix up. Do not.

- `DISPLAY_HISTOGRAMS_TO_STAFF` defaults to False. Its warning says histograms scan `courseware_studentmodule` on each view and can make staff courseware slow on large courses. Turning that on is not part of this check.
- `ENABLE_DISCUSSION_EMAIL_DIGEST` is a different flag. Do not flip it while you are here.

## On an LMS you operate

Print the resolved value from the LMS Django shell your release documents. On the `edx-platform` tree that is `manage.py lms shell`:

```python
from django.conf import settings
print(settings.DISPLAY_DEBUG_INFO_TO_STAFF)
```

`True` means the default is still in force, or an overlay set it back to True. Open one test course as a course-staff user and confirm whether Staff Debug is on the block. A global True with no button on that course is still worth recording. Do not use a learner account for the check.

To turn it off, set `DISPLAY_DEBUG_INFO_TO_STAFF` to False in the LMS settings overlay that your install already loads. Do not edit `common.py` on a running site and do not guess a Tutor path you have not opened. Restart the LMS process, print the setting again, and reload the test course as staff. The button should be gone. Histograms stay at their own default unless you already changed that flag.

I publish these notes while working on [GuardLMS](https://guardlms.com/en/integrations/open-edx). The inventory above is the procedure. It is not a claim that the product runs it for you.
