# Background tasks in build environments<a name="build-env-ref-background-tasks"></a>

You can run background tasks in build environments\. To do this, in your buildspec, use the `nohup` command to run a command as a task in the background, even if the build process exits the shell\. Use the disown command to forcibly stop a running background task\.

**Examples:**
+ Start a background process and wait for it to complete later:

  ```
  nohup sleep 30 & echo $! > pidfile
  …
  wait $(cat pidfile)
  ```
+  Start a background process and do not wait for it to ever complete:

  ```
  nohup sleep 30 & disown $!
  ```
+  Start a background process and kill it later:

  ```
  nohup sleep 30 & echo $! > pidfile
  …
  kill $(cat pidfile)
  ```