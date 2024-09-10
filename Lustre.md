---
tags:
  - storage
---
I keep most of my cryptic Lustre knowledge here: [Lustre pearls of wisdom](https://glennklockwood.com/data-intensive/storage/lustre-pearls.html).

## POSIX compliance

Lustre is not strictly POSIX compliant as many people claim. Though it does correctly handle multiple writers touching the same extent, exceptions to the standard exist in more obscure places:

- atime is only stored persistently at file close
- atime updates are persisted only once every sixty seconds
- Large writes that touch multiple OSTs may not be written atomically when concurrent writers touch the same offset in a file