Kerberos: we get a signed ticket from the KDC that permits us to access the requested resource

**Problem
When we use Kerberos to establish a remote session, we are not using a password for authentication. When password authentication is used, with PSExec, for example, that NTLM hash is stored in the session, so when we go to access another resource, the machine can pull the hash from memory and authenticate us.
