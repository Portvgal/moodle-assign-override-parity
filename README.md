# Moodle Assignment Override Attempts Patch

This repository contains a patch for Moodle core `mod_assign` that adds `Allowed attempts` to Assignment user and group overrides.

The patch is intended for Moodle `MOODLE_405_STABLE` and was tested locally on Moodle `4.5.11+`.

## Author

Patch prepared and tested by 
```text
@Portvgal [John Braz]
```

## Patch

Patch file:

```text
patches/assign-override-maxattempts.patch
```

Apply it from the root of a Moodle checkout:

```bash
git checkout -b test-assign-override-maxattempts
git apply /path/to/assign-override-maxattempts.patch
php admin/cli/upgrade.php
php admin/cli/purge_caches.php
```

## What It Adds

Assignment overrides can now set `Allowed attempts`, matching the Assignment setting of the same name.

The value means total attempts allowed, not extra attempts added.

Example:

```text
Assignment allowed attempts: 2
User override allowed attempts: 3
Result: that user may have 3 total attempts.
```

## Using Override Attempts With Assignment Settings

Override attempts work together with the Assignment `Attempts reopened` setting.

If `Attempts reopened` is set to `Manually`, increasing the override does not automatically create a new attempt. The teacher still needs to go back to the student's submission or grading screen and choose `Allow another attempt`.

Common trap:

```text
Assignment allowed attempts: 2
Attempts reopened: Manually
Student has submitted attempt 2
Teacher grades attempt 2
Teacher creates user override allowed attempts: 3
```

At this point the student still may not see a new attempt until the teacher explicitly allows another attempt. The override raises the limit; it does not reopen the submission by itself.

Practical workflow for manual reopening:

1. Confirm the Assignment `Attempts reopened` setting is `Manually`.
2. Create a user or group override with a higher `Allowed attempts` value.
3. Confirm the override dates still allow submission, especially the cut-off date.
4. Return to the student's submission or grading page.
5. Use `Allow another attempt`.
6. The student can then submit the newly opened attempt.

For several students, teachers can use the grading table bulk action after creating the override:

1. Create the user or group override first, including `Allowed attempts`, due date, and cut-off date.
2. Go to `View all submissions`.
3. Select the affected students in the grading table.
4. Use the bulk `Allow another attempt` action.

The bulk action only opens the next attempt for the selected students. It does not create an override and does not change `Allow submissions from`, due date, or cut-off date. If the cut-off date has already passed, update the user or group override dates before using the bulk action.

If `Attempts reopened` is `Automatically until pass` or another automatic mode, Moodle's normal reopen logic still controls when another attempt is created. The override only changes the maximum allowed attempt count used by that logic.

## User And Group Override Priority

Assignment overrides can be created for individual users and for groups.

Normal UI behavior is:

- One user override per assignment per user.
- One group override per assignment per group.
- A user may be affected by several group overrides if they belong to several groups.
- A user-specific override takes priority over group overrides.
- If multiple group overrides apply, Assignment uses the group override with the highest priority in the Group overrides list. In practice, this is the group override with the lowest sort order.

Important implication:

```text
Group A override allowed attempts: 4
Group B override allowed attempts: 5
Student belongs to both groups
```

The student does not automatically receive the most generous value. They receive the value from the group override that Assignment selects by group override order.

If a student needs a different result from their group, create a user override for that student. The user override is the clearest and safest way to handle individual special consideration cases.

## Tested

Local testing included:

- Manual testing of Assignment user and group overrides.
- Behat user override add/edit/delete scenario.
- Behat group override add/edit/delete scenario.
- PHPUnit tests for user override attempts, group override attempts, unlimited attempts, bulk grant attempt visibility, override resolution, dates, and backup restore.
- Moodle upgrade and cache purge checks.

Residual risk: the full `mod_assign` PHPUnit suite and full Assignment Behat suite were not run.
