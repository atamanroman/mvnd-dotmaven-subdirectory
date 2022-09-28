# mvnd Bug with `-f` and `.mvn` Directory Next to `pom.xml`

Expected behaviour:

mvn(d) loads the necessary  `--add-{exports,opens}` from `svc/.m2/jvm.config`, even with `-f svc/pom.xml`.

Actual behaviour:

mvn loads the necessary  `--add-{exports,opens}` from `svc/.m2/jvm.config`, even with `-f svc/pom.xml`. \
**mvnd does not.**

Context:

```
$ mvn --version
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /usr/local/Cellar/maven/3.8.6/libexec
Java version: 17.0.4.1, vendor: Eclipse Adoptium, runtime: /Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home
Default locale: en_DE, platform encoding: UTF-8
OS name: "mac os x", version: "12.6", arch: "x86_64", family: "mac"

$ mvnd --version
mvnd 0.8.1 darwin-amd64 native client (821c6a54a20ce4c4a2b94419334aaf125a128caf)
Terminal: org.jline.terminal.impl.PosixSysTerminal with pty org.jline.terminal.impl.jansi.osx.OsXNativePty
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /usr/local/Cellar/mvnd/0.8.1/libexec/mvn
Java version: 17.0.4.1, vendor: Eclipse Adoptium, runtime: /Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home
Default locale: en_DE, platform encoding: UTF-8
OS name: "mac os x", version: "12.6", arch: "x86_64", family: "mac"
```

Output with mvn in svc/ (GOOD):

```shell
repro/svc @ main
$ mvn --quiet verify && echo OK
OK
```

Output with mvnd in scv/ (GOOD):

```shell
repro/svc @ main
$ mvnd --quiet verify && echo OK
OK
```

Output with mvn -f svc/pom.xml (GOOD):

```shell
repro @ main
$ mvn --quiet -f svc/pom.xml verify && echo OK
OK
```

Output with mvnd -f svc/pom.xml (BAD):

```shell
repro @ main
$ mvnd --quiet -f svc/pom.xml verify && echo OK
[WARN] [stderr] An exception has occurred in the compiler (17.0.4.1). Please file a bug against the Java compiler via the Java bug reporting page (http://bugreport.java.com) after checking the Bug Database (http://bugs.java.com) for duplicates. Include your program, the following diagnostic, and the parameters passed to the Java compiler in your report. Thank you.
[WARN] [stderr] java.lang.IllegalAccessError: class com.google.errorprone.BaseErrorProneJavaCompiler (in unnamed module @0x35b09011) cannot access class com.sun.tools.javac.api.BasicJavacTask (in module jdk.compiler) because module jdk.compiler does not export com.sun.tools.javac.api to unnamed module @0x35b09011
...
[ERROR] COMPILATION ERROR :
[ERROR] An unknown compilation problem occurred
...
```
