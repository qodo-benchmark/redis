# Compliance Rules

This file contains the compliance and code quality rules for this repository.

## 1. All Source Files Must Include Tri-License Header

**Objective:** Ensure legal compliance and proper licensing attribution by requiring all C source and header files to include the standard Redis tri-license copyright header (RSALv2/SSPLv1/AGPLv3)

**Success Criteria:** Every .c and .h file in the src/ directory contains a copyright header with the text 'Licensed under your choice of (a) the Redis Source Available License 2.0 (RSALv2); or (b) the Server Side Public License v1 (SSPLv1); or (c) the GNU Affero General Public License v3 (AGPLv3)'

**Failure Criteria:** Any .c or .h file in src/ lacks the tri-license header or uses an incorrect license format

---

## 2. Use zmalloc/zfree Wrappers Instead of Direct malloc/free

**Objective:** Enable memory usage tracking and prevent memory leaks by enforcing the use of Redis's zmalloc family of functions (zmalloc, zcalloc, zrealloc, zfree) instead of standard library malloc/free functions

**Success Criteria:** All dynamic memory allocations in src/ use zmalloc, zcalloc, zrealloc, ztrycalloc, or ztrymalloc functions, and all deallocations use zfree

**Failure Criteria:** Code contains direct calls to malloc(), calloc(), realloc(), or free() outside of the zmalloc implementation itself

---

## 3. Header Files Must Use Double-Underscore Include Guards

**Objective:** Prevent multiple inclusion errors and maintain consistency by enforcing a standardized naming pattern for include guards in header files

**Success Criteria:** All .h files use include guards following the pattern #ifndef __FILENAME_H / #define __FILENAME_H / #endif, with double underscores at the start and end

**Failure Criteria:** Header files use include guards with non-standard naming (single underscore, no underscores, or different patterns)

---

## 4. Build Must Compile Without Warnings When -Werror is Enabled

**Objective:** Maintain high code quality and prevent potentially buggy code from being merged by ensuring the codebase compiles cleanly with compiler warnings treated as errors

**Success Criteria:** Running 'make REDIS_CFLAGS="-Werror"' completes successfully with no compilation warnings or errors

**Failure Criteria:** Compilation fails or produces warnings when -Werror flag is enabled

---

## 5. Test Functions Must Be Wrapped in REDIS_TEST Preprocessor Blocks

**Objective:** Separate test code from production code to reduce binary size and prevent test-only code from being included in production builds

**Success Criteria:** All test functions and test-specific code in source files are enclosed within #ifdef REDIS_TEST / #endif blocks

**Failure Criteria:** Test functions or test-specific code exists in source files without being wrapped in REDIS_TEST conditional compilation blocks

---

## 6. Redis Commands Must Have Corresponding JSON Definition Files

**Objective:** Ensure all Redis commands are properly documented and structured by maintaining JSON schema definitions in src/commands/ directory for command metadata, arguments, and behavior

**Success Criteria:** For every command implemented in the codebase, there exists a corresponding .json file in src/commands/ directory with complete command definition including summary, complexity, group, arity, function name, and reply schema

**Failure Criteria:** A command implementation exists without a corresponding JSON definition file, or the JSON file is incomplete or malformed

---

## 7. Internal Helper Functions Must Be Declared Static

**Objective:** Enforce proper encapsulation and prevent symbol pollution by ensuring functions that are not part of the public API are declared with static storage class

**Success Criteria:** Functions that are only used within a single source file are declared with the 'static' keyword

**Failure Criteria:** Helper functions or internal implementation functions are declared without 'static' keyword, making them visible to other compilation units unnecessarily

---

## 8. Generated commands.def Must Be Kept in Sync with Command JSON Files

**Objective:** Prevent inconsistencies between command definitions and generated code by ensuring the commands.def file is regenerated whenever command JSON files are modified

**Success Criteria:** After modifying any .json file in src/commands/, running 'make commands.def' produces no git diff in the commands.def file

**Failure Criteria:** Modifications to command JSON files result in a git diff when running 'make commands.def', indicating commands.def is out of sync

---
