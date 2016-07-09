# Introduction

This product is a simple easy-to-use tool which generates Remember The Milk tasks based on a task template.

# Mission

Some projects have tasks which are before or after a particular defined time.  An extreme example would be a space launch.  The rocket must be assembled, the payload must be delivered, the launch site must be reserved, these all must be done before takeoff -- but there are post-takeoff chores as well, such as in-flight monitoring and landing the payload.  Everything revolves around the launch date, which is often known years in advance.  If the launch date is moved, all the other associated tasks must move.

If these projects are recurring, it can become tedious to build these tasks by hand.  Having a pre-defined template of tasks and a tool to automatically create those tasks would make life easier.

# Scope

The major assumptions are that the RTM APIs will be stable, and that access to them will remain free.  We are not going to replace RTM -- this tool only adds and modifies tasks and lists.

# Interface

## Environment variables

An API key from RTM is required to use the tool.  It must be stored in an environment variable.  If the environment variable is not set, the tool will report an error.

An account with RTM is required to use the tool.  The username can be passed on the command line or stored as an environment variable.  If the username is not on the command line or stored as an environment variable, the tool will report an error.

## Task template

The user will generate a task template in a text file using the editor of their choice.  The template consists of comma-delimited lines having the following format:

```
Task name, Time offset, Description
```

An example template:

```
Reserve launch site, -2 weeks, Kerbin Space Center is nice this time of year
Accept payload deliver, -1 week, Check the blinking lights
Assemble rocket, -1 day, Moar boosters!
Launch!, 0 days, This is when the magic happens
Health check, 90 minutes, First orbit
Download data, 2 weeks, Science mission is complete
Deorbit, 2 weeks 1 day, Drop it in the water
```

The time offsets can be whatever Go can handle for time offsets.

## Usage examples

The user has a template named science-mission.txt in the current directory and they wish to schedule a launch for 2016 Aug 6.  The command line will look like this:

`$ gophermilk add "Science Mission Number 1" -t science-mission.txt -z “2016 Aug 6 12:00” -u rtmusername`

The user will be asked for their password.  The user’s Remember The Milk account will be accessed, a new list named "Science Mission Number 1" will be created, and new tasks will be created based on the contents of the template, with the zero time being noon on August 6th, 2016.

The user decides to reschedule the launch for the following week.  The command line will look like this:

`$ gophermilk modify "Science Mission Number 1" -t science-mission.txt -z “2016 Aug 13 15:00” -u rtmusername`

Again, the user will be asked for their password.  This time, the as-yet-uncompleted tasks on the list will have their dates changed based on the new zero time.

If the user wishes to cancel the launch, the list should be deleted via the web site.

In all cases, errors will be reported:

* If authentication fails
* If files cannot be read
* If dates are not sane
* If lists exist (add) or do not exist (modify)

## Command line

### Arguments

	Event name	String		Unchecked

### Flags

	-t	Template	Filename	Must be normal file, not stdin.
	-z	Zero time	Datetime	Whatever format Go can handle.
	-u	Username	String		Only required if not in environment variable.
