# Splunk Basic Notes

## What is Splunk?

Splunk is a SIEM (Security Information and Event Management) platform.

A SIEM collects, indexes, and searches log data from different systems so analysts can investigate activity from one central location instead of checking every system individually.

Basic workflow:

System generates logs
→ Splunk ingests the logs
→ Splunk indexes the data
→ Analyst searches the data
→ Analyst investigates activity

---

## Key Splunk Terminology

### Index
An index is a top-level container where Splunk stores related data.

Example:

index=main

This tells Splunk to search data stored in the `main` index.

### Host
The host identifies the machine or system associated with the event.

Example:

host="SalesData"

### Source
The source identifies where the data came from, such as a file path, script, or data input.

### Sourcetype
The sourcetype describes what kind of data Splunk is processing, such as a web access log, Linux authentication log, or CSV file.

### Event
An event is one individual record in Splunk.

For example, one failed login attempt could be one event.

### Field
A field is a searchable piece of information extracted from an event.

Examples:

- IP
- Platform
- Genre
- Rating
- Message
- host

### Time
Time identifies when the event occurred.

Splunk uses timestamps so analysts can investigate when activity happened.

---

## Search & Reporting

The Search & Reporting app is where SPL searches are written and run.

SPL stands for:

Search Processing Language

Splunk searches can start broad and then be narrowed using fields and commands.

Example:

index=main host="SalesData"

This searches the `main` index for events associated with the `SalesData` host.

---

## Search Modes

Splunk has different search modes.

### Smart Mode
Optimizes searches and may limit some field extraction.

### Verbose Mode
Shows more field information.

Verbose Mode is useful during investigation when I want to inspect all available fields.

---

## Interesting Fields

The Interesting Fields panel shows fields Splunk extracted from the events.

I can click fields such as:

- Genre
- Rating
- Platform
- IP
- Message

to see their values and use them to narrow a search.

---

## Filtering by Fields

Example:

index=main host="SalesData" Genre="Action"

This searches only for events where the Genre field is Action.

Multiple filters can be combined.

Example:

index=main host="SalesData" Genre="Action" Rating="E"

---

## Wildcards

The `*` symbol is a wildcard.

It means match any value.

Example:

Platform=*

This matches events that have any value in the Platform field.

---

## Pipes

The pipe symbol:

|

passes the results from one command into the next command.

Example:

index=main host="SalesData"
| stats count by Platform

The first part finds the events.

The `stats` command then processes those results.

---

## stats

`stats` summarizes data.

Example:

index=main host="SalesData"
| stats count by Platform

This groups the events by Platform and counts how many events belong to each platform.

---

## sort

Example:

| sort -count

This sorts the `count` field from highest to lowest.

The minus sign means descending order.

---

## head

Example:

| head 5

This returns only the first 5 results.

If I want the top 5 results, I should normally sort first.

Example:

index=main host="SalesData"
| stats count by Platform
| sort -count
| head 5

Logic:

1. Count events by Platform
2. Sort the counts from highest to lowest
3. Keep the top 5

---

## Visualizations

Splunk can turn search results into visualizations such as:

- Pie charts
- Bar charts
- Column charts
- Tables

The visualization represents the results returned by the SPL search.

For example, if a search only returns the top 5 platforms, the pie chart only represents those five platforms.

---

## Dashboards

Dashboards combine multiple searches and visualizations into one view.

I created a dashboard called:

Video Game Statistics

It included panels such as:

- Top 5 Gaming Platforms
- Top Genres
- Rating

Dashboards are useful for monitoring information quickly without rebuilding the same searches repeatedly.

---

## Reports

Reports are saved searches that can be run again later.

They can also be used for recurring monitoring.

I created a security-focused report showing the top source IP addresses generating failed login attempts.

---

## Security Data Practice

I searched authentication data from:

host="WebServer01"

Basic search:

index=main host="WebServer01"

Then I filtered for failed logins:

index=main host="WebServer01" Message="Failed password for"

This searches only for events where the Message field indicates a failed password attempt.

---

## Finding Top Failed-Login IPs

Search:

index=main host="WebServer01" Message="Failed password for"
| stats count by IP
| sort -count

What it does:

1. Searches WebServer01 authentication events
2. Filters for failed login attempts
3. Groups the failed attempts by source IP
4. Counts how many attempts came from each IP
5. Sorts the IPs from highest number of failures to lowest

This can help identify an IP generating an unusually large number of failed login attempts.

---

## Important Investigation Lesson

A large number of failed logins from one IP can be suspicious, but failed logins alone do not prove that an attacker successfully accessed the system.

An analyst should investigate further before making a conclusion.

---

## SPL Searches Practiced

### Search all SalesData events

index=main host="SalesData"

### Count events by platform

index=main host="SalesData"
| stats count by Platform

### Find the top 5 platforms

index=main host="SalesData"
| stats count by Platform
| sort -count
| head 5

### Count events by genre

index=main host="SalesData"
| stats count by Genre
| sort -count

### Search WebServer01 authentication logs

index=main host="WebServer01"

### Search failed login events

index=main host="WebServer01" Message="Failed password for"

### Rank source IPs by failed login count

index=main host="WebServer01" Message="Failed password for"
| stats count by IP
| sort -count

---

## What I Learned

After completing CodePath Splunk Lab Part 1, I can:

- Explain the basic purpose of a SIEM
- Navigate Splunk Search & Reporting
- Understand index, host, source, sourcetype, event, field, and time
- Inspect extracted fields
- Filter events using fields
- Use wildcards
- Understand SPL pipes
- Use `stats`
- Use `sort`
- Use `head`
- Create visualizations
- Create dashboards
- Save reports
- Search authentication data
- Identify IP addresses generating repeated failed login attempts

Next step:

Ingest Windows Security logs from my own Windows computer into Splunk and begin working with my own security event data.