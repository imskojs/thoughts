# Angular thoughts/notes/gotchas

* Components in `entryComponents` array are not tree shaked (this means non-entry components are tree shaked even if it is defined in the `declarations` array if not used). Components defined in Router, or defined in `bootstrap` array have the same effect as defining them in `entryComponents` array.
