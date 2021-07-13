Shout out to https://linuxize.com/post/how-to-run-linux-commands-in-background/

## Running a command in the background
To run a command in the background, add the ampersand symbol (&) at the end of the command:
```
$ command &
```
The shell job ID (surrounded with brackets) and process ID will be printed on the terminal:

```
[1] 25177
```
The background process will continue to write messages to the terminal from which you invoked the command. To suppress the stdout and stderr messages use the following syntax:
```
$ command > /dev/null 2>&1 & 
```
`>/dev/null 2>&1` means redirect stdout to `/dev/null` and stderr to stdout .

Use the jobs utility to display the status of all stopped and background jobs in the current shell session:
```
$ jobs -l
```
To bring a background process to the foreground, use the fg command:
```
$ fg
```
If you have multiple background jobs, include % and the job ID after the command:
```
$fg %1
```
To terminate the background process, use the kill command followed by the process ID:
```
kill -9 25177
```

## Keep Background Processes Running After a Shell Exits
One way is to remove the job from the shell’s job control using the disown shell builtin:
```
$ disown
```
If you have more than one background jobs, include % and the job ID after the command:
```
$ disown %1
```

Confirm that the job is removed from the table of active jobs using the jobs -l command. To list all running processes, including the disowned use the ps aux command.

Another way to keep a process running after the shell exit is to use nohup.

The nohup command executes another program specified as its argument and ignores all SIGHUP (hangup) signals. SIGHUP is a signal that is sent to a process when its controlling terminal is closed.
```
$ nohup command &
```
The command output is redirected to the nohup.out file.
```
### Output
nohup: ignoring input and appending output to 'nohup.out'
```
