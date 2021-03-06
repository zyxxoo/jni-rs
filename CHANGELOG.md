# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

<!-- Use the following sections from the spec: http://keepachangelog.com/en/1.0.0/
  - Added for new features.
  - Changed for changes in existing functionality.
  - Deprecated for soon-to-be removed features.
  - Removed for now removed features.
  - Fixed for any bug fixes.
  - Security in case of vulnerabilities. -->

## [Unreleased]

### Added
- Invocation API support on Windows and AppVeyor CI (#149)

### Changed
- `push_local_frame`, `delete_global_ref` and `release_string_utf_chars`
no longer check for exceptions as they are
[safe](https://docs.oracle.com/javase/10/docs/specs/jni/design.html#exception-handling)
to call if there is a pending exception (#124):
  - `push_local_frame` will now work in case of pending exceptions — as
  the spec requires; and fail in case of allocation errors
  - `delete_global_ref` and `release_string_utf_chars` won't print incorrect
  log messages

- Rename some macros to better express their intent (see #123):
  - Rename `jni_call` to `jni_non_null_call` as it checks the return value
  to be non-null.
  - Rename `jni_non_null_call` (which may return nulls) to `jni_non_void_call`.

- A lot of public methods of `JNIEnv` have been marked as safe, all unsafe code 
  has been isolated inside internal macros. Methods with `_unsafe` suffixes have
  been renamed and now have `_unchecked` suffixes (#140)

- `from_str` method of the `JavaType` has been replaced by the `FromStr`
  implementation

- Implemented Sync for GlobalRef (#102).

- Improvements in macro usage for JNI methods calls (#136):
  - `call_static_method_unchecked` and `get_static_field_unchecked` methods are 
  allowed to return NULL object
  - Added checking for pending exception to the `call_static_method_unchecked` 
  method (eliminated WARNING messages in log)
  
-  Further improvements in macro usage for JNI method calls (#150):
  - The new_global_ref() and new_local_ref() functions are allowed to work with NULL objects according to specification.
  - Fixed the family of functions new_direct_byte_buffer(), get_direct_buffer_address() and get_direct_buffer_capacity()
   by adding checking for null and error codes.
  - Increased tests coverage for JNIEnv functions.

- Implemented Clone for JNIEnv (#147).

### Fixed
- The issue with early detaching of a thread by nested AttachGuard. (#139)

## [0.10.2]

### Added
- `JavaVM#get_java_vm_pointer` to retrieve a JavaVM pointer (#98)
- This changelog and other project documents (#106)

### Changed
- The project is moved to an organization (#104)
- Updated versions of dependencies (#105)
- Improved project documents (#107)

### Fixed
- Crate type of a shared library with native methods 
  must be `cdylib` (#100)

## [0.10.1]
- No changes has been made to the Changelog until this release.

[Unreleased]: https://github.com/jni-rs/jni-rs/compare/v0.10.2...HEAD
[0.10.2]: https://github.com/jni-rs/compare/v0.10.1...v0.10.2
[0.10.1]: https://github.com/jni-rs/compare/v0.1...v0.10.1
