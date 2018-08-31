# wb_gl_loader
## by William Bundy

A single-header OpenGL loader, using win32 or SDL as a backend out-of the box.

Also handles context creation on Win32 (because nobody wants to do that).

## Sample usage:


	#define WB_GL_USE_ALL_VERSIONS
	#define WB_GL_WIN32
	#define WB_GL_LOADER_IMPLEMENTATION
	#include "wb_gl_loader.h"
	...
	{
		wbgl_win32_create_context(windowDC, 3, 3, 1);
		wbgl_load_all(NULL);
	* }

## Versions

For simplicity, I handle OpenGL versions in groups:
 - legacy (2.1 and previous)
 - compat (3.1)
 - core (3.2 and 3.3)
 - modern (4.0 - 4.5)

These map on to several defines:

- `WB_GL_USE_LEGACY`
- `WB_GL_USE_COMPAT`
- `WB_GL_USE_CORE`
- `WB_GL_USE_MODERN`

Or, you can just use `WB_GL_USE_ALL_VERSIONS`

## Defining your loader function

If you want to use a built-in loader, use either:

- `WB_GL_WIN32`
- `WB_GL_SDL`

If you don't define either of those, no loader function will be defined. 
You can define your own like so:

```
WB_GL_LOADER_API
void* wbgl__load_proc(const char* name, struct wbgl_ErrorContext* ctx, void* userdata)
{
	void* proc = MyGetProcAddress(name);
	if(!proc) {
		wbgl__load_proc_error(name, ctx);
	}
	return proc;
}
```

## Error Handling

Supplied to the load proc is a struct that keeps track of the first `WB_GL_MAX_ERRORS` (default: 256) functions that fail to load. If you pass one of these `struct wbgl_ErrorContext` into `wbgl_load`, it'll tell you which functions failed to load. 

However, it's largely safe to ignore unloaded functions; if you don't call them, everything will be fine.


