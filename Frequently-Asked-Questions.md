# Are catch-all aliases supported?

Yes, catch-all aliases are supported as part of *wildcard* aliases. To enable *wildcard* aliases, simply create an alias and enable the "Use SQL LIKE syntax" feature. This will enable two wildcards: ``_`` (underscore) which stands for any single character and ``%`` (percent) which stands for any substring.

Example aliases include:
 - ``%`` (per-domain catch-all)
 - ``%master`` (will include ``postmaster``, ``hostmaster``, ``newsmaster``, etc.)
 - ``a_`` (any two-letters localpart starting with an a)

# Can a user mailbox forward to multiple destinations

No, not for now. If you need a mailbox to forward to multiple destinations, either set an alias that redirects to these destinations and include a mailbox in the target emails, or setup an alias in the mailbox automatic forward. Issue #34 is open about this.

# How do I activate DKIM?

Go into the Domain Panel, choose the Domain you want to enable DKIM for, click the first Icon on the left side, and click on the top right on the "Regenerate Keys" Button. It will generate the DKIM Entry for you.

# I am running CoreOS and Freeposte.io does not seem to work properly

CoreOS does not come with the latest Docker API (written in october 2016), you can either select another operating system or upgrade to the ``alpha`` release that meets our requirements.