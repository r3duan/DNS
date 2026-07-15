# The Hosts File

The hosts file is basically DNS's manual override switch. Before your computer ever bothers asking a resolver anything, most operating systems check this local file first — so if a name is listed there, DNS never even gets consulted. That's the key thing that makes it powerful: it's not just another cache, it's a shortcut that takes priority over the entire DNS chain we walked through earlier (resolver → root → TLD → authoritative).

**Where it lives:**

- Windows: `C:\Windows\System32\drivers\etc\hosts`
- Linux/macOS: `/etc/hosts`

**Format** — dead simple, one mapping per line:

```
<IP Address>    <Hostname> [<Alias> ...]
```

So `127.0.0.1 localhost` tells your machine "don't look this up anywhere, `localhost` is `127.0.0.1`, full stop."

**Why it needs admin/root privileges to edit:** it's a trust boundary. Anything that controls this file controls where your traffic for a given domain actually goes — which is exactly why malware often tries to write to it, and why it requires elevated permissions to modify.

**Changes apply instantly** — no restart, no service reload, no waiting on a TTL to expire. That immediacy is part of what makes it so handy.

## The three practical uses it mentions

1. **Local development** — point a domain your app expects (`myapp.local`) at `127.0.0.1` so you can test against "the real domain name" without owning that domain or touching any actual DNS records.

  ```
  127.0.0.1       myapp.local
  ```
2. **Connectivity testing** — map a hostname straight to a known IP (`testserver.local` → `192.168.1.20`) to isolate whether a problem is a DNS issue or an actual network/server issue. If it works via the hosts file but not through normal DNS, you've just proven DNS is the broken link.
      ```
      192.168.1.20    testserver.meow
      ```

3. **Blocking sites** — redirect an unwanted domain to `0.0.0.0` (a non-routable address), so any attempt to reach it just fails instead of connecting anywhere. This is the same trick a lot of ad-blocking hosts-file lists use at scale.

      ```bash
      0.0.0.0       unwanted-site.com
      ```
