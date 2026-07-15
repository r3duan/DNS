# How DNS Works: The Journey From Domain Name to IP Address

DNS is the system that turns human-friendly names (like `www.reduan.com`) into machine-friendly IP addresses. Nobody owns the whole thing — it's not one giant database somewhere, but thousands of servers around the world each holding a small piece of the puzzle and cooperating to answer your query. Think of it less like a single phone book and more like a chain of increasingly specific phone books, where each one tells you which more specific book to check next.

There are several types of DNS servers that are used worldwide:

- DNS root server
- Authoritative name server
- Non-authoritative name server
- Caching server
- Forwarding server
- Resolver

## How a Query Actually Resolves — The Pieces Involved

Different servers play different roles in that chain:

1. **Your computer looks for the address first (the initial query)**
   The moment you type a domain into your browser, your machine doesn't go straight to the internet — it checks its own local cache first, in case it already resolved that name recently. If nothing's there, it hands the job off to a DNS resolver, which is normally run by your ISP.

2. **The resolver goes hunting (recursive lookup)**
   The resolver checks its own cache too. If it comes up empty, it starts working through the DNS hierarchy on your behalf, starting with a query to one of the root name servers — the top of the whole system.

3. **The root server redirects, it doesn't answer**
   A root server has no idea what IP your domain maps to, but it does know which TLD server is responsible for that domain's ending (`.com`, `.org`, `.net`, etc.), and sends the resolver there next.

4. **The TLD server gets more specific**
   The TLD server doesn't have the IP either, but it does know exactly which authoritative name server owns your particular domain, and hands the resolver off one level further.

5. **The authoritative server has the real answer**
   This is the end of the chain — the server that actually holds the record for your domain. It's like the street address of the website you want. It holds the correct IP address and sends it back to the resolver.

6. **The resolver hands it back — and remembers it**
   The resolver passes the IP address along to your computer, and caches it locally for a while so the next lookup for that same domain doesn't have to repeat the whole journey.

7. **Your computer finally connects**
   With the IP address in hand, your browser opens a direct connection to that web server, and the page starts loading.

## Resolver

Now you might think here at this point: what is a resolver? Let me clear it for you.

A resolver is the piece of software that actually does the legwork of a DNS lookup on your behalf — it's not authoritative for anything itself, it just knows how to go ask the right servers in the right order and hand you back an answer.

There are really two resolvers involved in that whole process, and it's easy to blur them together:

1. **The stub resolver — runs on your own machine**
   This is a small piece of software built into your OS or browser. It's not smart — it doesn't walk the DNS hierarchy itself. Its whole job is to check the local cache and, if nothing's there, forward the question to a bigger, smarter resolver (usually configured by your ISP or manually, like `8.8.8.8` or `1.1.1.1`).

2. **The recursive resolver — the one that does the actual hunting**
   This is the resolver from steps 2–6 in the walkthrough above. It's the one that:
   - Checks its own (much larger) cache first, since it's answering this same query for thousands of other users too.
   - If it's a cache miss, goes and queries the root server, then the TLD server, then the authoritative server — chasing referrals down the hierarchy until it gets a real IP address.
   - Caches that answer for the TTL the authoritative server specified, so the next person asking for the same domain gets an instant answer.
   - Sends the final IP back to your stub resolver.

---

*Written by Reduan Islam Badhon*
