---
title: Support
---

# Support: Business Days & Holidays for Jira

Email **[{{ site.support_email }}](mailto:{{ site.support_email }})**. We reply within 2 business days.

{% raw %}
## Quick start

1. **Jira settings → Apps → Business Days & Holidays.** Set up the **Default** calendar: pick a country, a state or province if needed, and your time zone.
2. In an Automation rule, add **Add business days**. Start date `{{now}}`, days `5`. The first time anyone on your site adds one of the app's actions, Jira asks you to **connect** Automation to the app. Accept it once and you're set.
3. Add **Edit work item** and set **Due date** to `{{fetchedJiraBusinessday.date}}`.

## Smart values

| Action | Smart value | Fields |
|---|---|---|
| Add business days | `{{fetchedJiraBusinessday}}` | `date` (YYYY-MM-DD), `dateTime` (ISO with offset; keeps the start's time of day), `skippedDays`, `skippedHolidays`, `cutoffApplied`, `calendar` |
| Count business days | `{{fetchedJiraBdcount}}` | `count` (negative if *To* is before *From*), `calendar` |
| Check business day | `{{fetchedJiraBdcheck}}` | `isBusinessDay`, `reason` (`weekend` / `holiday` / `closure` / empty), `name`, `nextBusinessDay`, `previousBusinessDay`, `calendar` |

Use them as `{{<smart value>.<field>}}`, for example `{{fetchedJiraBdcount.count}}` or `{{fetchedJiraBdcheck.isBusinessDay}}`. Jira picks these names, and you can't rename them in the rule.

## Troubleshooting

**The result is one day off.** You probably passed a date-only field as a timestamp. Use `{{issue.duedate.jiraDate}}` rather than `{{issue.duedate}}`. Also check the calendar's time zone, because timestamps such as `{{now}}` are converted to it first.

**A holiday is missing or wrong.** Holiday data comes from the open-source date-holidays project. Add the day as a **closure**, or untick it under *Holidays your team takes off*. Tell us and we'll get it fixed upstream.

**A holiday on a Saturday isn't moved to Monday.** Substitute rules differ by employer. Add the observed day as a closure.

**The rule fails with a permission error, or the actor shows as "deleted user".** This comes from Jira, not the app. On new sites the default rule actor, *Automation for Jira*, sometimes lacks permission in a new space. In the rule details, set **Actor** to *User who triggered the event*, or give the default actor edit permission in that space.

**"No calendar called X".** The Calendar field takes a calendar *key* or *name*. Leave it empty to use the project's calendar or the default.

**"License … inactive".** A Jira admin can renew under *Apps → Manage apps*.

## How counting works

- Fri + 1 business day = Mon. Sat + 1 = Mon. Adding 0 gives today if it's a business day, otherwise the next business day.
- Counting from Mon to Tue = 1. Fri to Mon = 1. The start day is never counted.
- If a **cutoff** is set (for example 17:00), work that arrives after it counts from the next day.
{% endraw %}

[Privacy policy](privacy) · [End user terms](terms)
