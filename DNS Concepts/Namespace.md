# What Is a Namespace?
 
A namespace is just a system for organizing names so that each one is unique within its own boundary — a set of rules for how names are structured and who's allowed to claim which ones, so two different things don't accidentally end up with the same identifier.
 
The core idea: names only need to be unique within their namespace, not universally. `Charlie` in your contacts list and `Charlie` in someone else's contacts list can coexist just fine — they're different namespaces. But two contacts named `Charlie` in your own list would be ambiguous — same namespace, collision.
 
## Namespaces in DNS
 
In DNS specifically (since that's what we were using the term for): the whole domain name system is one giant hierarchical namespace. It's organized as a tree, and each level delegates authority over unique names to the level below it:
 
- Under the root, `.com` is unique — there's only one `.com` TLD.
- Under `.com`, `reduan.com` has to be unique — nobody else can register that exact second-level domain.
- Under `reduan.com`, the owner of that domain can create `www`, `mail`, `dev`, etc., and those just need to be unique within `reduan.com` — someone else's domain can also have a `www` subdomain, no conflict, because it lives in a different branch of the tree.
