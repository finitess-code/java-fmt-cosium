# Java formatting (Cosium Maven plugin + Google formatter)

This is an example on how to format Java code on a `pre-commit` hook using `git-code-format-maven-plugin` from Cosium
and `google-java-format` formatter from Google.

Cosium `git-code-format-maven-plugin` automatically detects Maven and installs a pre-commit Git hook. The hook will
run `google-java-format` formatter before commit is made.

For more up-to-date information
check [Cosium Git Code Format Maven Plugin official page](https://github.com/Cosium/git-code-format-maven-plugin).

## Java 16+

For Java version 16 and above additional launcher options are required

```text
--add-exports jdk.compiler/com.sun.tools.javac.api=ALL-UNNAMED 
--add-exports jdk.compiler/com.sun.tools.javac.file=ALL-UNNAMED 
--add-exports jdk.compiler/com.sun.tools.javac.parser=ALL-UNNAMED 
--add-exports jdk.compiler/com.sun.tools.javac.tree=ALL-UNNAMED 
--add-exports jdk.compiler/com.sun.tools.javac.util=ALL-UNNAMED
```

They can be passed to Maven in a `jvm.config` configuration file.

Check [these release notes](https://github.com/google/google-java-format/releases/tag/v1.10.0).

## Validate formatting manually

```shell
./mvnw git-code-format:validate-code-format -Dgcf.globPattern="**/*"
```

## Format code manually

```shell
./mvnw git-code-format:format-code -Dgcf.globPattern="**/*"
```

## Formatting outcomes

Before formatting:

```java
package com.finitess.fmtcosium;

import java.util.*;
import java.util.stream.Collectors;
import java.nio.*;
import org.springframework.beans.factory.annotation.Value;
import java.util.UUID;


public
class  UnformattedClassA
{

    void        hello() {
//        this should return hello
        System.
                out
                .println("hello");

        List.of("1", "2",     "3").stream().map( Integer::parseInt    )

                .filter(number -> number % 2 == 0).

                collect(Collectors.toSet());

    }

}

class SomeInnerClassA


{

}
```

After formatting:

```java
package com.finitess.fmtcosium;

import java.nio.*;
import java.util.*;
import java.util.stream.Collectors;

public class UnformattedClassA {

    void hello() {
        //        this should return hello
        System.out.println("hello");

        List.of("1", "2", "3").stream()
                .map(Integer::parseInt)
                .filter(number -> number % 2 == 0)
                .collect(Collectors.toSet());
    }
}

class SomeInnerClassA {}

```

### Pros

- Incremental formatting on older projects (only files that were changed will be formatted)
- Integrates both with installed Maven and Maven wrapper
- Auto-installed git hooks on `initialize` goal
- Integrates with existing hooks (just adds a line to the actual hook)
- Minimal formatting configuration set

### Cons

- Does not seem to detect/remove wildcard unused imports
- Does not support other formatters (for example, Palantir)
- Created `#!/bin/bash` hook is not platform independent, should be `#!/usr/bin/env bash`