# Porting Plan for Python 3.2.5

This document outlines a phased approach for porting the `docopt` package
and its test suite to run on Python **3.2.5** while preserving existing
functionality. Each phase is a small, self‑contained work unit suitable for
Codex.

## Phase 1 – Baseline Assessment

1. Install Python **3.2.5** and create a virtual environment.
2. Run the current tests under 3.2.5 to establish a baseline.  `tox.ini`
   already defines a `py32` environment:
   ```ini
[tox]
envlist = py27, py32, py34, py35, py36
skip_missing_interpreters = True
```
3. Record any failures or errors for follow‑up.

## Phase 2 – Source Compatibility Review

1. Audit `docopt.py` and tests for features introduced after Python 3.2,
   such as `yield from` or f‑strings.
2. Remove `__future__` imports that are unnecessary on Python 3, e.g. the
   first line of `test_docopt.py`:
   ```python
from __future__ import with_statement
```
3. Replace or backport incompatible language features.
4. Add small compatibility modules for any missing standard‑library
   functionality.

## Phase 3 – Packaging Adjustments

1. Update `setup.py` to state the supported Python version using
   `python_requires` and keep the Python 3.2 classifier:
   ```python
        'Programming Language :: Python :: 3.2',
```
2. Remove classifiers for versions that will no longer be supported
   (such as Python 2) if narrowing to 3.2 only.
3. Pin `pytest` or other test dependencies to versions that still support
   Python 3.2 and update `tox.ini` accordingly.

## Phase 4 – Test Suite Updates

1. Modify any tests that rely on newer features so they run on
   Python 3.2.5 unchanged.
2. Run `tox -e py32` until the suite passes completely.
3. Update documentation; README currently lists supported versions:
   ```
**docopt** is tested with Python 2.7, 3.2, 3.4, 3.5, and 3.6.
```
   Adjust this to mention Python 3.2.5 specifically.

## Phase 5 – Continuous Integration

1. Add a CI job that installs Python 3.2.5 and runs the suite via `tox`.
2. Ensure CI environments use only dependency versions compatible with
   Python 3.2.

## Phase 6 – Final Verification and Release

1. Execute the full test matrix locally and in CI to confirm stability.
2. Tag and publish a release (source archive and wheel) built under
   Python 3.2.5.

Following these steps will yield a version of `docopt` fully compatible
with Python 3.2.5 while maintaining its existing behaviour and test
coverage.
