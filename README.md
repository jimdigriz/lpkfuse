# lpkfuse - SSH Public Keys in LDAP using FUSE

## Configuration Files

### /etc/ssh/lpkfuse
    
    [server]
    ;uri      = ldap.example.com
    ;uri      = ldap1.example.com:1234, ldap2.example.com
    uri       = ldap://ldap1.example.com, ldaps://ldap2.example.com:3389, ldapi:///
    
    ;username = "cn=username,ou=accounts,dc=example,dc=com"
    ;password = the-biggest-secret
    
    ; timeout in seconds
    ;timeout  = 5
    
    [user]
    base    = "ou=accounts,dc=example,dc=com"
    filter  = (objectClass=posixAccount)
    key     = cn

### /etc/fstab
    
    #lpkfuse#       /opt/lpkfuse    fuse    ro,config=/etc/ssh/lpkfuse,debug         0  0
    #lpkfuse#       /opt/lpkfuse    fuse    ro,config=/etc/ssh/lpkfuse,verbose       0  0
    lpkfuse#        /opt/lpkfuse    fuse    ro,config=/etc/ssh/lpkfuse               0  0

**N.B.** Debian users should look at [Debian Bug #526115 - [fuse-utils] fuse entries in fstab are not mounted automatically](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=526115).

### /etc/ssh/sshd_config
    
    AuthorizedKeysFile      /opt/lpkfuse/%u

### /etc/rc.local

It is worth configuring the OOM system to pass over the daemon for obvious reasons:
    
    echo -17 > /proc/$(pgrep lpkfuse)/oom_adj || true

## Links

 * [OpenSSH-LPK](http://code.google.com/p/openssh-lpk/)
 * [openssh-lpk_openldap.schema](http://openssh-lpk.googlecode.com/files/openssh-lpk_openldap.schema)
 * [fuselpk](http://pypi.python.org/pypi/fuselpk/) - a similar Python implementation
