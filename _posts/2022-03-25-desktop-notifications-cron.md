---
layout: post
title:  "Sending Desktop Notifications from a Cron Job"
category: engineering
---

Let’s say you have a cron job defined like this:

<br />

{% highlight bash %}
* * * * * /my-script.sh
{% endhighlight %}

<br />

And within `my-script.sh`, you want to:

* Run some command
* Send a desktop notification if that command fails

<br />

Here’s how to do it:

{% highlight bash %}
#!/bin/bash

set -o errexit -o errtrace

function error_notify {
  XDG_RUNTIME_DIR=/run/user/$(id -u) notify-send -u critical "Cron Task Failed" "Error Details"
}
trap 'error_notify' ERR

a-very-crony-command
{% endhighlight %}

<br />

Here’s the breakdown:

* **set -o errexit** - early exit script if any command fails and trigger the trap ERR
* **set -o errtrace** - also trigger the trap ERR if any functions or subshells fail
* **XDG_RUNTIME_DIR=/run/user/$(id -u)** - notify-send needs this environment variable set to function properly
* **trap ‘error_notify’ ERR** - execute the error_notify function if an ERR is triggered

<br />

Most Linux distributions have `notify-send` installed, but if not, you’ll need to install it.  On Debian-based systems, you can it install with:

{% highlight bash %}
sudo apt install libnotify-bin
{% endhighlight %}

<br />

Here’s what the final result looks like on my system (Pop_OS! 21.10):

![Cron Task Notification](/assets/images/cron-task-notification.png)