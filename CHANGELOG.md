# Changelog

## [1.1.0] - 2026-10-17

### Added

The release 1.1.0 adds a new argument `--gui` to the tool, if this argument is passed, the tool with be started in GUI mode.

## [1.0.0] - 2026-10-10
The script was completely overhauled/refactored, and I did several code optimizations. Furthermore, I've added some new functionality and fixed several bugs.

After several tests, I decided to release this script now as stable v1.0.0.

### Added

New options:
- Multiple binary-log files can now be analyzed concurrently (option: `-j`, `--jobs`)
- Path to `mysqlbinlog` can be specified (option: `--mysqlbinlog`)
- Pre-filter of log files can be omitted (option `--ignore-mtime`)
- Sample SQL queries can be written to separate report files (option: `-q`, `--write-queries`)
- Number of sample SQL queries can be specified (option: `--query-samples`)
- Colored terminal output can be disabled (option `--no-color`)

### Fixed

- `checkWorkingParameters`: The help text promises today 23:59:59. Seconds aren’t zeroed either.
- `dtFrom > dtTo` check: The message arguments are swapped: it prints `dtTo` as “start” and `dtFrom` as “end”.
- `getopt` long options: The list has `output-file=` but the code checked `--output-path`, so that option raised an unhandled GetoptError. The list had `skip-gtid-Strict-Mode`, but the code checked `--no-skip-gtid-strict-mode`, so only `-g` worked. The usage text also said `--dt-end`, but the real option is `--dt-to`.
- `getBinaryLogs`: root + file had no path separator for subdirectories (it only worked for the top level because the base path ends in /).
- `getBinaryLogs` regex: `\.[0-9]*$` also matched mysql-bin.
- `createReport`: Fixed an issue, where white-spaces in a path can lead to a shell-injection
- `createReport`: `sqlOperations` was lower-cased only if it contained a comma, so `--sql-operations=UPDATE` was rejected.
- `decodeAndCombineBinaryLogs`: If `mysqlbinlog` was not installed, FileNotFoundError (a subclass of OSError/IOError) was caught by except IOError and reported as a problem with the output file.
- `decodeAndCombineBinaryLogs`: If `mysqlbinlog` failed with an empty stderr, only a warning was shown and the script continued, so the report may have silently been built from partial output. The warning’s `.format(error)` also had no placeholder.
- `run`: `createOutputFiles` was running before `getBinaryLogs`. If no logs matched, `sys.exit(0)` left three empty files behind.
- Date parsing: Invalid input (e.g. `--dt-from=yesterday`) silently fell back to today instead of resulting in an error.

## [0.0.5] - 2026-05-02

### Added

- Added option to disable the `mysqlbinlog` argument `--skip-gtid-strict-mode`

## [0.0.4] - 2024-12-07

### Added

- Added a warning if `mysqlbinlog` exited with status code 1, but without an error message.

## [0.0.3] - 2024-09-30

### Fixed

- Error in sorting bin logs with binlognnnnnn filename pattern.

## [0.0.2] - 2023-11-14

### Added

- Missing option to usage output

## [0.0.1] - 2023-11-13

Initial public release
