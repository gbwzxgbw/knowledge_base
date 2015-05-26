#reserved space for root
    Saving space for important root processes (and possible rescue actions) is one reason.
    But there's another. Ext3 is pretty good at avoiding filesystem fragmentation, but once you get above about 95% full, that behavior falls off the cliff, and suddenly filesystem performance becomes a mess. So leaving 5% reserved gives you a buffer against this.
    Ext4 should be better at this, as explained by Linux filesystem developer/guru Theodore Ts'o:
        If you set the reserved block count to zero, it won't affect performance much except if you run for long periods of time (with lots of file creates and deletes) while the filesystem is almost full (i.e., say above 95%), at which point you'll be subject to fragmentation problems. Ext4's multi-block allocator is much more fragmentation resistant, because it tries much harder to find contiguous blocks, so even if you don't enable the other ext4 features, you'll see better results simply mounting an ext3 filesystem using ext4 before the filesystem gets completely full.
        If you are just using the filesystem for long-term archive, where files aren't changing very often (i.e., a huge mp3 or video store), it obviously won't matter.

#systemd
    http://dynacont.net/documentation/linux/Useful_SystemD_commands/ 
    https://wiki.debian.org/systemd 
    https://www.linux.com/learn/tutorials/527639-managing-services-on-linux-with-systemd 
    
#debian version
    lsb_release -a 
    cat /etc/issue 
    cat /etc/debian_version 
    
#kernel version
    uname -a
    
#locale
    http://perlgeek.de/en/article/set-up-a-clean-utf8-environment
