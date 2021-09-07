# Scheduler and Tasks

## Introduction

Task files are JSON files (with name format `YYYY-MM-DD-id-N.json`) placed in
`application/tasks`. These files will be created and deleted automatically.
On the application server start all files will be loaded into a special thread
to be executed.

How to schedule a task:
```js
application.scheduler.add({
  name: 'name',
  every: 'Jul 22th 100s',
  args: { i: 2 },
  run: 'lib.task1.f1',
});
```

## Task files

Task file fields:
- `id: string` - task unique identifier (example: `"2021-07-22-id-0"`);
- `name: string` - not unique task name;
- `every: string` - (example: `Jul 22th 100s`), see format below;
- `args: object` - task arguments (example: `{"i":2}`);
- `run: string` - function name to run (example: `lib.task1.f1`);

Task file example:
```js
{
  "id": "2021-07-22-id-0",
  "name": "name",
  "every": "Jul 22th 100s",
  "args": { "i": 2 },
  "run": "lib.task1.f1"
}
```

## Syntax for `every`

- `Apr 1st` - once at `00:00` 1st of April
- `15th 100s` - every 100 seconds each 15th
- `Sun 17:` - every Sunday at `17:00`
- `20th 17:15` - every 20th of any month at `17:15`
- `Apr Sun :30` - at HH:30 every hour on Sunday of April
- `2nd :30` - every 1st of any month at HH:30 every hour
- `Sun 3rd 00:` - every Sunday if this day will be 3rd of month at `00:00`
- `30m 17s` - interval 30 minutes and 17 seconds
- `Sun 5h 30m` - every Sunday with interval 5 hours 30 minutes
- `2021-07-20` - certain date
- `2021-07-20 17:30` - certain date and time
