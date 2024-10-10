# papi

Prompt API (PAPI) provides abstractions between LLMs and structured data sources.  Rather than using LangChain to integrate directly with Google Calendar, PAPI providers calendar interfaces that allow extensible code to be created that will integrate not only with Google Calendar, but any calendar.  This interface is like other LLM to SQL generators, but designed not just to read, but also to update the data source.  This extensible structure allows for custom modules and RHLF allows the prompts to adaptively improve.

PAPI aspires to implement using the pareto principle.  We will not support all features, but will support 80% of features.

PAPI is like a prompt to SQL generator specifically for models defined in this schema.

# How

papi can/fetch or write to a data store.  Think of it like GraphQL.  With resolvers for interacting with n-number of data stores.

papi guarantees type safety.

[Your business logic] <=> [PAPI] <=> [Merge] <=> [Google Calendar]

# Why

* You can think of PAPI like an ORM that allows mapping between a set of well structured prompts and structured data sources.  This is like an ORM from the early 2000s.

# Flow
* user: create a new salesforce activity
* agent: fetch schema from papi
* agent: collect info
* agent: return proposed changeset (dryrun)
* user: RHFL
* user: modify/approve
* agent: execute

# Agent flow
* agent: search for scheme matching "salesforce activity"
* agent: return schema from PAPI schema registry
* SFActivity
```
type SFActivity = {
  account: SFAccount
  activity_date: date
  activity_type: enum
  activity_subject: string
  activity_notes: text
}
```
agent: please tell me which customer you spoke with, when you spoke with them, and what you discussed?
user: I met with Perplexity yesterday and discussed their RDS instance optimization.  They're planning to shard their data.
agent: great, here's the proposed changeset, look ok?
```
type SFActivity = {
  account: 'Perplexity'
  activity_date: 2024-10-8
  activity_type: Arch Review
  activity_subject: Discuss RDS performance challenges
  activity_notes: Discussed RDS performance challenges.  Planning to shard data.
}
* user: looks good
* agent: it's logged

# How it's built?
* Uses https://github.com/vanna-ai/vanna
* https://github.com/Dataherald/dataherald
* https://github.com/Sinaptik-AI/pandas-ai
* https://www.e2enetworks.com/blog/how-to-use-llms-to-interact-with-your-sql-data

# Initial standard schema object types

# Email
* from, to, cc, bcc, status [draft, sent], subject, body

# Event
* parent_calendar, datetime, duration, subject, location, invitees, notes

# Calendar
* name

# Note
* title, created_at, updated_at, body, tags

# Salesforce Activity

# Web page
* url, title, content, available_inputs :list, available_actions :list

# Todo/Reminder
* name, status, state, due_at, created_at, updated_at

# Example uses

# Example arch

* pg_vector


## Reminder me to discuss an issue tomorrow with my car dealer
```
prompt: "Remind me to talk to the car dealer about the sunroof tomorrow"
PAPI: preprocess prompt and perform entity extraction

**classify task**: "remind me" => intent detected (reminder, task)
**action detection**: "talk to" => action
**party**: "car dealer" => entity
**subject**: "sunroof" => subject
**timing**: "tomorrow" => today + 1.days

agent: "Search for appointment <TOMORROW> related to <CAR>"
Planner:
* Classifier ==> Reminder
* Calendar => semantic search
* Reminders => semantic search
* Notes => semantic search
PAPI: semantic search based on calendar embeddings for <CAR>
agent: "I found an appointment, "Drop car At Ford".  I'm planning to append this to the existing notes in your appointment tomorrow titled <TITLE>.  Confirm or something else?"
[option path 1]
user: "something else"
agent: "Would you like to create a reminder?"
user: "yes"
agent: "it's done"
[option path 2]
user: "yes"
agent: "it's done, I've added your note -'discuss sunroof' to your calendar appointment tomorrow"
```

## What's on my calendar tomorrow?
* fetch calendar
* classify: schedule question
* time bounding: tomorrow
* search: Calendar events
* read back subjects

# Email john about hanging out Friday
* check calendar for openings Friday
* draft message
* confirm message
* send message


