# killptree : Kill process tree

## Purpose

Kill all processes that are descendants of (i.e. children, grandchildren , great grandchildren etc.) any given process.

## Use-case

May be called by handlers of `exit` traps in shell-scripts for graceful shutdown of background processes such as daemons.

## Usage

```shell
killptree [-s signal] [-k seconds] [-w] pid # When run as executable

. ./killptree; kill_proc_tree [-s signal] [-k seconds] [-w] pid # When sourced
```

`-s`: Pass the signal to be sent. Default: SIGTERM

`-k`: Pass number of seconds to wait before sending SIGKILL. Default: 0.1

`-w`: Wait for the descendant processes to be killed before killing the given process. Default: No wait

## Example

To kill all processes belonging to a process group as well as their descendants: 

```shell
pgrep -g "${pid_of_pgroup_leader}" | xargs -n1 killptree
```

## Test

```shell
(sleep 100 | (sleep 200 | ( sleep 300 | sleep 400 ) ) ) & pid=$!
ps -ef
killptree $pid
echo Happy?
```

