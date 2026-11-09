# Configuration walkthrough

These snippets are drawn from the sanitized examples already in this repository. They demonstrate specific implementation choices, not a ready-to-deploy configuration. Replace example entities and endpoints for your own installation.

## Overnight shifts cross the date boundary

[Source: work-week script](projects/home-assistant-work-week/scripts.yaml)

```yaml
end_date: >-
  {% if end_time <= start_time %}
    {{ (as_datetime(shift_date) + timedelta(days=1)).date() }}
  {% else %}
    {{ shift_date }}
  {% endif %}
end_value: "{{ end_date ~ ' ' ~ end_time }}"
```

The script combines the selected shift date with the start and end times. When the end time is earlier than or equal to the start time, it uses the following date for the end. A 22:00–06:00 shift therefore ends the next morning rather than before it starts. Equal start and end times are treated as a full-day span by this condition, not a zero-length event.

The complete script creates a timed calendar entry and a separate all-day display entry, requests a dashboard refresh, and resets the time and notes helpers. Its comments describe a confirmation popup handled by the dashboard before this write script is called; the script itself is not a confirmation mechanism.

## Chore payloads use structured JSON encoding

[Source: Donetick REST command](projects/home-assistant-chores/rest-command.yaml)

```yaml
headers:
  Authorization: !secret donetick_authorization_header
  Content-Type: application/json
```

The authorization value is referenced from Home Assistant's secrets file rather than embedded in the published example. The example URL uses a placeholder host and port.

Inside the JSON payload:

```jinja
"name": {{ chore_name | tojson }},
"frequencyType": {{ frequency_type | tojson }},
"frequency": {{ frequency | int }},
"dueDate": {{ due_date | tojson }},
```

The `tojson` filter encodes text values as JSON, including quotation marks and escapes. Numeric fields are converted to integers. This is safer than manually wrapping interpolated text in quotes, but it is not a substitute for validating inputs.

The source documents an integration-specific lesson: this Donetick API version expects `dueDate` on creation. Using `nextDueDate` produced rolling chores with no stored due date. Confirm your deployed API version before adapting the example.

## Screenshot policy

The project screenshot directories currently contain placeholders rather than a verified gallery. Useful future captures would show the work-week workflow, chore form, and dashboard using demo entities and sample schedules. Do not publish household schedules, access tokens, private addresses, or personal records to illustrate a feature.

These are configuration explanations, not a claim that a new live deployment or end-to-end test was performed.
