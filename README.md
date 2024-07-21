# papi

Prompt API (PAPI) provider abstractions between LLMs and structured data sources.  Rather than using LangChain to integrate directly with Google Calendar, PAPI providers calendar interfaces that allow extensible code to be created that will integrate not only with Google Calendar, but any calendar.

PAPI aspires to implement using the pareto principle.  We will not support all features, but will support 80% of features.

# Initial object types

# Email
* from, to, cc, bcc, status [draft, sent], subject, body

# Event
* parent_calendar, datetime, duration, subject, location, invitees, notes

# Calendar
* name

# Note
* title, body

# Web page
* url, title, content, available_inputs :list, available_actions :list

# Todo/Reminder
* name, datetime

# Example uses

# Example arch

* pg_vector


## Reminder me to discuss an issue tomorrow with my car dealer
```
prompt: "Remind me to talk to the car dealer about the sunroof tomorrow"
PAPI: preprocess prompt and perform entity extraction
agent: "Search for appointment <TOMORROW> related to <CAR>"
Planner:
* Events => semantic search
* Reminders => semantic search
* Notes => semantic search
PAPI: semantic search based on calendar embeddings for <CAR>
agent: "I found an appointment, "Drop car At Ford".  I'm planning to append this to the existing notes in your appointment tomorrow titled <TITLE>.  Confirm or something else?"
[branch 1]
user: "something else"
agent: "Create a reminder?"
user: "yes"
agent: "it's done"
[branch 2]
user: "yes"
agent: "it's done"
```

## What's on my calendar tomorrow?
* fetch calendar
* read back subjects

# Email john about hanging out Friday
* check calendar for openings Friday
* draft message
* confirm message
* send message


