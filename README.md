# slax-busybox
Unlock all the bash commands in slax.  Zero install.

A lot of the commands in slax are consolidated into /usr/bin/busybox.  However, not every command is exposed directly to the user.  For example, to use vi you would have to type busybox vi.

All these extra commands can be made available by simply creating a symbolic link.  There is no code to install.

The following command will create all the missing links for /usr/bin/busybox.
<pre>cd /usr/local/bin
for Each in $(/usr/bin/busybox --list); do
   which $Each || ln -fs /usr/bin/busybox $Each
done</pre>
There is a second busybox in initramfs which is used during the boot sequence.  As this is not using any space, we may as well include these commands also.
<pre>cp /run/initramfs/bin/busybox /usr/local/sbin/
cd /usr/local/sbin
for Each in $(./busybox --list); do
   which $Each || ln -fs busybox $Each
done</pre>
You can savechanges to build a module so the commands are retained if you boot to a clean session.
