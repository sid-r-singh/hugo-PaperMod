---
author: "Sidharth"
title: "Understanding the Rsync Command"
date: "2021-05-24"
description: "Using rsync in linux"
tags: ["bash", "linux"]
categories: ["bash"]
aliases: ["understand rsunc"]
ShowToc: true
TocOpen: false
---

### Example command

    rsync -ahvz user@production_server:/path/to/source/content /path/to/local/backup/storage/

### rsync


For a full details of rsync, [check out the manual page](http://linux.die.net/man/1/rsync).


A basic rsync command pattern:

    rsync copyfrom copyto

The file or directory you want to back up is `copyfrom`, and `copyto` is the place you want to store the backup. `Copyfrom` and `copyto` are arguments of the rsync command and are required. An rsync command with basic `copyfrom` and `copyto` arguments would look like this:

    rsync user@production_server:~/public ~/backups/mybackup

    |---| |-----------------------------| |----------------|
      ^                 ^                          ^
      |                 |                          |
    rsync           copyfrom                    copyto

Rsync can also run with additional options, which are included in the command before the main arguments:

    rsync --options copyfrom copyto

### -ahvz

Here are some standard options for rsync:

    -ahvz

These are four rsync options that have been combined into a single directive. You can also list them out individually like this:

    -a -h -v -z

These options have the following effects:

-   -a or --archive: Preserves our file permissions and ownership, copies recursively, etc.
-   -h or --human-readable: Number outputs are human readable
-   -v or --verbose: Displays more output
-   -z or --compress: Compress file data during transfer

You can add or remove any of the rsync options. For example, if you don't need to see all of the output, you could just run:

    -ahz

When creating backups, the essential option is `-a` or `--archive`.

### Source Location

The `copyfrom` location is the path to what you want to back up on your *production\_server*. This is where you should put the file path to your content on the server.

    user@production_server:~/public
    |--------------------| |------|
              ^                ^
              |                |
         SSH login           path

Since you're trying to copy from a remote server (the *production\_server*), you should provide the SSH login credentials first. Then use a colon (`:`), followed by an absolute file path to the folder you want to back up.

In this example, you're backing up the `~/public` directory, which is where your websites should be located if you followed the [Hosting a Website](/docs/websites/hosting-a-website/) guide. `~` is a shortcut for `/home/user/`. The trailing `/` is omitted from the final directory because you want to include the `public` folder itself in the backup, not just its contents.

If you want to do a full-server backup from root, you should use `/*` as your path. You should also exclude some folders from the backup so you don't get a lot of warnings and errors every time you run it. `/dev`, `/proc`, `/sys`, `/tmp`, and `/run` do not contain permanent data, and `/mnt` is the mount point for other file systems. To make an exclusion, add the `--exclude` option at the very end of the rsync command, after everything else.

    --exclude={/dev/*,/proc/*,/sys/*,/tmp/*,/run/*,/mnt/*}

You will also need to use either `root` or a sudo-capable user for the backup, if you are backing up from `/*` or another high-level directory. If you use a sudo user, you will need to either disable the need for the password when rsync is used, or send the password to the server. The [crashingdaily blog](https://crashingdaily.wordpress.com/2007/06/29/rsync-and-sudo-over-ssh/) has a good discussion of this.

### Target Location

The `copyto` location is the path to where you want to store your backup on your *backup\_server*.

    ~/backups/mybackup

In the command for automatic backups (see below), a date variable is appended to the file path:

    ~/backups/public_$(date -I)
                     |--------| <- date variable
    |-------------------------|
                 ^
                 |
               path

This is the local file path on the *backup\_server* where you want to store the backups. The variable `$(date -I)` uses the built-in `date` function to add the current date to the end of the file path. This makes sure a new folder is created for each backup, and also makes individual backups easy to find.

### Cron

The following command extends the previous example to enable automatic backups and use some advanced options:

    0   3   *   *   *   rsync -ahvz --delete --link-dest=~/backups/public_orig user@production_server:~/public ~/backups/public_$(date -I)

The series of numbers and asterisks specifies when the cron task should be run (remember, this command was added to the `crontab` file [earlier in this guide](#set-up-automatic-backups-to-a-linux-server)).

    0       3           *           *       *        Command

    ^       ^           ^           ^       ^           ^
    |       |           |           |       |           |

    Minute Hour     Day of Month  Month  Weekday   Shell command

For each of the five time categories you can specify either a specific number or `*`, which means "every." Use the 24-hour clock for hours. The example above specifies that the command should run on the first minute of the third hour of every day (3 am). Anything you add after the fifth number or asterisk is considered the actual command, which will be run just as if you had typed it in your shell. You can [read more about cron here](http://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/).

For testing purposes, you can set the task to run at `* * * * *`, which will create a new backup every minute. Running backups like this may use your allotted transfer, so running a new one every minute could result in overage charges.
