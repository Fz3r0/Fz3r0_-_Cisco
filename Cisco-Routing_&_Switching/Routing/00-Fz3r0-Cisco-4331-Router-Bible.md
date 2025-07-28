

````py
## Mandatory

! # 1. Erase the startup configuration so the router boots with no config
write erase

! # 2. Reload the router to apply the wipe and start fresh
reload


## Optional

! # -. Check the flash storage for leftover files (old configs, backups, etc.)
dir flash:

! # -. Manually delete any unwanted files you find in flash
delete flash:<filename>

! # -. (Optional) Clear any stored license information to leave the router completely clean
license clear


````



## Resources

- https://www.youtube.com/watch?v=bk-4jkb6Cwo
- https://www.youtube.com/watch?v=CwfFQxd2zig
