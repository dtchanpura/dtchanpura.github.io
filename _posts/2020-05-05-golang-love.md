---
layout: post
title: I am in Love with Go
date: 2020-05-05T00:00:00.000Z
updated_on: 2020-05-05T00:00:00.000Z
categories: technology
status: beta
description: >-
  Go can be efficient in comparison with basic Bash commands.
display: true
tags:
  - devops
---

# I am in love with Go :heart_eyes:

Recently due to a bug in our configuration (and lack of monitoring) the number of uWSGI log files were increasing every few minutes. We realized that it was mainly because of max log file size. Instead of adding it in 1000000 we just put 10MB assuming it took MB as input as well but it did not take that and by default it went up and took, 1kilo-byte and the log which it had was just "triggering rotation to filename".

This configuration was added a year ago and as expected we had about 3 Million files in one folder and with no cleaning mechanism added as such it would grow a lot and eventually fill up the disk space.

Eventually we changed the configuration and restarted the service which started to work as expected, no more files and no more logs like "triggering rotation to filename". But still another problem remains. A lot of files in a folder, with some other logs as well. The file pattern was `/var/log/uwsgi/uwsgi.log.%s` where `%s` is the unix timestamp.

Now as a linux user I thought let’s clean this up, and I did `rm -rf /var/log/uwsgi/*` and it just threw an error saying `unable to execute /bin/rm: Argument list too long`, and I was thinking of options and came across two possible solutions I could follow.

1. Try to delete in chunks as 
```sh
for i in `seq 15090 15500`
do
	rm -f /var/log/uwsgi/uwsgi.log.${i}*
done
```
2. Other option was to use the find command 
```sh
find /var/log/uwsgi -type f -name 'uwsgi.log.15*' -exec rm \-f {} \;
```

But both of them took a long time. Then I remembered there was something like a walk function in Go. My IDE helped a lot and I never even had to do a search on how to use it.

Following piece of code helped me delete files that I wanted to.

```go
func deletePath(path, prefix string) error {
	return filepath.Walk(path, func(pathstr string, info os.FileInfo, err error) error {
		if strings.HasPrefix(pathstr, filepath.Join(path, prefix)) {
			err := os.Remove(pathstr)
			if err != nil {
				return err
			}
		}
		return nil
	})
}
```

This does not feel any different than removing files sequentially. But combined all that with concurrency it helps a lot. So this function might be just deleting files which match the code but eventually we do this in parallel.

Finally I created the tool and built a binary for Linux and put it in the production server and ran this and it just deleted all files under a minute. As a benchmark it deleted about 300k files in ~16 seconds.

Also to note the server where I ran this code was a 6 core machine so the performance was supported by the hardware, even though the server was a CentOS 6 with Kernel 2.6.

