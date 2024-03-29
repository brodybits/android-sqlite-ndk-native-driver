# Android SQLite NDK native driver (...)

Provides Android NDK build of sqlite3 (<http://sqlite.org/>, public domain) with a low-level JNI interface accessible from static methods of `SQLiteNDKNativeDriver` class _along with the following extra features: REGEXP, BASE64_ ... Intended for use with: [`brodybits / android-sqlite-native-ndk-connector`](https://github.com/brodybits/android-sqlite-native-ndk-connector)

Based on:

- [`liteglue / Android-sqlite-native-driver`](https://github.com/liteglue/Android-sqlite-native-driver)
- [`sqlg / SQLiteGlue-core`](https://github.com/sqlg/SQLiteGlue-core)

by Christopher J. Brody aka Chris Brody mailto: <chris.brody+brodybits@gmail.com>

with some complex API response type enhancements by Luis Silva of OutSystems (luis dot silva at outsystems dot com)

**LICENSE:** [Unlicense (unlicense.org)](http://unlicense.org/) (public domain)

## About

provides the following items:

- `SQLiteNDKNativeDriver` class with native Java interface to an important subset of sqlite3 C functions
- `SQLiteNativeResponse` class with complex API response type
- automatic build of `sqlite-ndk-native-driver.jar` and `sqlite-ndk-native-driver-libs.zip` with sqlite3 component for major Android NDK targets (`armeabi-v7a` / `x86` / `x86_64` / `arm64-v8a`), with sqlite3 component _along with the following user defined functions:_
  - `REGEXP` integrated from [brodybits / sqlite3-regexp-cached](https://github.com/brodybits/sqlite3-regexp-cached) (based on <http://git.altlinux.org/people/at/packages/?p=sqlite3-pcre.git> by Alexey Tourbin, public domain)
  - `BASE64` and `BLOBFROMBASE64` integrated from [brodybits / sqlite3-base64](https://github.com/brodybits/sqlite3-base64), using [brodybits / libb64-core](https://github.com/brodybits/libb64-core) (based on <http://libb64.sourceforge.net/> by Chris Venter, public domain)

This was accomplished with some help from using [GlueGen](http://jogamp.org/gluegen/www/) around a simple wrapper C module.

This project is meant to help build a higher-level sqlite interface library, with the JNI layer completely isolated.

Minimum API level: android-22 (Android 5.1)

**NOTE:** This project references multiple subprojects, which may be resolved by: $ `make init` (as described below).

**WARNING:** The SQLite database and statement handles that are returned in `SQLiteNativeResponse` are raw C pointer values. If someone uses a database or statement handle that is not valid, or no longer valid then the behavior would be undefined (may crash, for example). It is **not** recommended to use the API directly without understanding of how this library works internally.

## SQLite build information

### SQLite version

    3.32.3

### android-ndk version notes

See the following reference for installing older `android-ndk` cask using Homebrew: <https://www.jverdeyen.be/mac/downgrade-brew-cask-application/>

To install `android-ndk` version `r16b`, for example:

```sh
brew cask install https://raw.githubusercontent.com/Homebrew/homebrew-cask/4570652dc6a3a8f7fd2be1053dd43547a2c78e26/Casks/android-ndk.rb
```

Note that `homebrew-cask` seems to have restored maintenance of the `android-ndk` cask, as discussed in [`Homebrew/homebrew-cask#58883`](https://github.com/Homebrew/homebrew-cask/issues/58883).

See also for some historical `android-ndk` cask information:

- https://github.com/Homebrew/homebrew-cask/commits/master/Casks/android-ndk.rb
- https://github.com/Homebrew/homebrew-cask/commits/5e9f77552aef2ffa29efe8a9b916d89686b96c7f/Casks/android-ndk.rb
- https://github.com/Homebrew/homebrew-cask/blob/5e9f77552aef2ffa29efe8a9b916d89686b96c7f/Casks/android-ndk.rb

FUTURE TODO: better documentation of API and some internal details

### SQLite build flags

- `-DSQLITE_THREADSAFE=1`
- `-DSQLITE_DEFAULT_SYNCHRONOUS=3`
- `-DSQLITE_DEFAULT_MEMSTATUS=0`
- `-DSQLITE_OMIT_DECLTYPE`
- `-DSQLITE_OMIT_DEPRECATED`
- `-DSQLITE_OMIT_PROGRESS_CALLBACK`
- `-DSQLITE_OMIT_SHARED_CACHE`
- `-DSQLITE_TEMP_STORE=2`
- `-DSQLITE_OMIT_LOAD_EXTENSION`
- `-DSQLITE_ENABLE_FTS3`
- `-DSQLITE_ENABLE_FTS3_PARENTHESIS`
- `-DSQLITE_ENABLE_FTS4`
- `-DSQLITE_ENABLE_FTS5`
- `-DSQLITE_ENABLE_RTREE`
- `-DSQLITE_ENABLE_JSON1`

New stable default page size and cache size (<https://sqlite.org/pgszchng2016.html>):

- `-DSQLITE_DEFAULT_PAGE_SIZE=4096`
- `-DSQLITE_DEFAULT_CACHE_SIZE=-2000`

## Dependencies

- SQLite (<https://sqlite.org/>) - public domain
- [brodybits / sqlite3-regexp-cached](https://github.com/brodybits/sqlite3-regexp-cached) - based on <http://git.altlinux.org/people/at/packages/?p=sqlite3-pcre.git> by Alexey Tourbin, public domain
- [brodybits / sqlite3-base64](https://github.com/brodybits/sqlite3-base64) - Unlicense (public domain) ref: <http://unlicense.org/>
- [brodybits / libb64-core](https://github.com/brodybits/libb64-core) - based on <http://libb64.sourceforge.net/> by Chris Venter, public domain

## Major TODOs and limitations

- Automatic AAR build
- Document this project (again, perhaps in a blog post)
- Some more SQLite API functions will be needed to rebuild the native sqlcipher library to replace the native libraries in the [@sqlcipher / android-database-sqlcipher](https://github.com/sqlcipher/android-database-sqlcipher) ([SQLCipher for Android](https://www.zetetic.net/sqlcipher/sqlcipher-for-android/)) project.

# Building

## Normal build

Initialize with the `gluegentools` and `sqlite-amalgamation` subprojects:

$ `make init`

Then to build:

$ `make`

## Regenerage Java & C glue code

$ `make regen`

# Testing

Tested with: [`brodybits / android-sqlite-native-ndk-connector`](https://github.com/brodybits/android-sqlite-native-ndk-connector)
