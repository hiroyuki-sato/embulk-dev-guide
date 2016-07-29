.. Embulk Plugin Developer Guide documentation master file, created by
   sphinx-quickstart on Tue Jul 19 21:00:20 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Embulk Plugin Developer Guide's documentation!
=========================================================

This is Embulk plugin developer guide.


This guide is designed for developers who want to create Embulk plugin.

You need to have some prerequisites installed

- OS

  + UNIX compatible OSs (Linux,OSX)
  + Windows (cygwin environment required)

- JDK >= Java7
- Publish plugin.

  + Rubygems account
  + Gem in a box (https://github.com/geminabox/geminabox) (for internal use)


This guide is based on Embulk 0.8.9. Another version may not work properly. If you find any problem, Please let us know.


Embulk Plugin development overview
===================================

You can develop Embulk plugin with Java or Ruby(JRuby) languages.

Some developers use Scala(Functional programming language on JVM environment) language.
If you want to develop Embulk plugin with Scala, Please refer Appendix pages.

The following list is developmenet language and plugin matrix.

+-------------+------+------+
| plugin      | Java | Ruby |
+=============+======+======+
| input       | x    | x    |
+-------------+------+------+
| file-input  | x    | \-   |
+-------------+------+------+
| output      | x    | x    |
+-------------+------+------+
| file-output | x    | \-   |
+-------------+------+------+
| parser      | x    | x    |
+-------------+------+------+
| formatter   | x    | x    |
+-------------+------+------+
| decoder     | x    | \-   |
+-------------+------+------+
| encoder     | x    | \-   |
+-------------+------+------+
| executor    | x    | x    |
+-------------+------+------+

.. note::

  intput and input-file, output output-file



Java Plugin Tutorial
====================

First, execute ``embulk new java-input java_example``\ command for generating input plugin templates.

::

    embulk new java-input java_example
    2016-07-17 22:07:09.046 +0900: Embulk v0.8.9
    Creating embulk-input-java_example/
      Creating embulk-input-java_example/README.md
      Creating embulk-input-java_example/LICENSE.txt
      Creating embulk-input-java_example/.gitignore
      Creating embulk-input-java_example/gradle/wrapper/gradle-wrapper.jar
      Creating embulk-input-java_example/gradle/wrapper/gradle-wrapper.properties
      Creating embulk-input-java_example/gradlew.bat
      Creating embulk-input-java_example/gradlew
      Creating embulk-input-java_example/config/checkstyle/checkstyle.xml
      Creating embulk-input-java_example/config/checkstyle/default.xml
      Creating embulk-input-java_example/build.gradle
      Creating embulk-input-java_example/lib/embulk/input/java_example.rb
      Creating embulk-input-java_example/src/main/java/org/embulk/input/java_example/JavaExampleInputPlugin.java
      Creating embulk-input-java_example/src/test/java/org/embulk/input/java_example/TestJavaExampleInputPlugin.java

    Plugin template is successfully generated.
    Next steps:

      $ cd embulk-input-java_example
      $ ./gradlew package

This command creates the following files.

::

    .
    |-- LICENSE.txt
    |-- README.md
    |-- build.gradle
    |-- config
    |   `-- checkstyle
    |       |-- checkstyle.xml
    |       `-- default.xml
    |-- gradle
    |   `-- wrapper
    |       |-- gradle-wrapper.jar
    |       `-- gradle-wrapper.properties
    |-- gradlew
    |-- gradlew.bat
    |-- lib
    |   `-- embulk
    |       `-- input
    |           `-- java_example.rb
    `-- src
        |-- main
        |   `-- java
        |       `-- org
        |           `-- embulk
        |               `-- input
        |                   `-- java_example
        |                       `-- JavaExampleInputPlugin.java
        `-- test
            `-- java
                `-- org
                    `-- embulk
                        `-- input
                            `-- java_example
                                `-- TestJavaExampleInputPlugin.java

Next change directory to ``embulk-input-java_example``\ directory, and execute ``./gradlew classpath``\ command(Linux,OSX), ``gradlew.bat classpath``\ (Windows).

::

    ./gradlew classpath
    :compileJava
    warning: [options] bootstrap class path not set in conjunction with -source 1.7
    1 warning
    :processResources UP-TO-DATE
    :classes
    :jar
    :classpath

    BUILD SUCCESSFUL

    Total time: 7.495 secs

    This build could be faster, please consider using the Gradle Daemon: https://docs.gradle.org/2.10/userguide/gradle_daemon.html

TODO: This plugin throw exception,

Ruby
~~~~

First, execute ``embulk new ruby-input ruby_example``\ command for generating input plugin templates.


::

    embulk new ruby-input ruby_example
    2016-07-17 21:29:16.803 +0900: Embulk v0.8.9
    Creating embulk-input-ruby_example/
      Creating embulk-input-ruby_example/README.md
      Creating embulk-input-ruby_example/LICENSE.txt
      Creating embulk-input-ruby_example/.gitignore
      Creating embulk-input-ruby_example/Rakefile
      Creating embulk-input-ruby_example/Gemfile
      Creating embulk-input-ruby_example/.ruby-version
      Creating embulk-input-ruby_example/embulk-input-ruby_example.gemspec
      Creating embulk-input-ruby_example/lib/embulk/input/ruby_example.rb

    Plugin template is successfully generated.
    Next steps:

      $ cd embulk-input-ruby_example
      $ bundle install                      # install one using rbenv & rbenv-build
      $ bundle exec rake                    # build gem to be released
      $ bundle exec embulk run config.yml   # you can run plugin using this command

This command creates the following files.

::

    .
    |-- Gemfile
    |-- LICENSE.txt
    |-- README.md
    |-- Rakefile
    |-- embulk-input-ruby_example.gemspec
    `-- lib
        `-- embulk
            `-- input
                `-- ruby_example.rb

    3 directories, 6 files

Next, create ``conf.yml`` file like the following.

.. code:: yaml

    in:
      type: ruby_example
      option1: 123

    out:
      type: stdout

.. note::

  Generated plugin require ``option1``, config. but this value does not use yet.

Execute the command ``embulk run -I embulk-input-ruby_example/lib conf.yml``.
``-I embulk-input-ruby_example/lib`` is the location of plugin lib directory.
With ``-I plugin/lib`` option, You can test plugin behavior without install the plugin.

The execution result is the following.

::

    embulk run -I embulk-input-ruby_example/lib conf.yml
    2016-07-17 21:30:51.441 +0900: Embulk v0.8.9
    2016-07-17 21:30:53.963 +0900 [INFO] (0001:transaction): Loaded plugin embulk/input/ruby_example from a load path
    2016-07-17 21:30:54.025 +0900 [INFO] (0001:transaction): Using local thread executor with max_threads=8 / output tasks 4 = input tasks 1 * 4
    2016-07-17 21:30:54.047 +0900 [INFO] (0001:transaction): {done:  0 / 1, running: 0}
    example-value,1,0.1
    example-value,2,0.2
    2016-07-17 21:30:54.164 +0900 [INFO] (0001:transaction): {done:  1 / 1, running: 0}
    2016-07-17 21:30:54.236 +0900 [INFO] (main): Committed.
    2016-07-17 21:30:54.236 +0900 [INFO] (main): Next config diff: {"in":{},"out":{}}

``Embulk new``\ コマンドがサンプルで生成するファイルは、options1の値を入力データとして利用していません。そこでソースコードを修正して、入力データにoptions1の値を利用するように修正をしましょう。



.. code:: diff

    --- a/lib/embulk/input/ruby_example.rb
    +++ b/lib/embulk/input/ruby_example.rb
    @@ -46,8 +46,8 @@ module Embulk
           end

           def run
    -        page_builder.add(["example-value", 1, 0.1])
    -        page_builder.add(["example-value", 2, 0.2])
    +        page_builder.add(["example-value", @option1, 0.1])
    +        page_builder.add(["example-value", @option1, 0.2])
             page_builder.finish

             task_report = {}

修正後、\ ``embulk run``\ コマンドを実行すると、options1に指定した値を出力していることが確認できます。

::

    embulk run -I embulk-input-ruby_example/lib conf.yml
    2016-07-17 21:33:20.908 +0900: Embulk v0.8.9
    2016-07-17 21:33:23.234 +0900 [INFO] (0001:transaction): Loaded plugin embulk/input/ruby_example from a load path
    2016-07-17 21:33:23.281 +0900 [INFO] (0001:transaction): Using local thread executor with max_threads=8 / output tasks 4 = input tasks 1 * 4
    2016-07-17 21:33:23.307 +0900 [INFO] (0001:transaction): {done:  0 / 1, running: 0}
    example-value,123,0.1
    example-value,123,0.2
    2016-07-17 21:33:23.414 +0900 [INFO] (0001:transaction): {done:  1 / 1, running: 0}
    2016-07-17 21:33:23.500 +0900 [INFO] (main): Committed.
    2016-07-17 21:33:23.500 +0900 [INFO] (main): Next config diff: {"in":{},"out":{}}




Contents:

.. toctree::
   :maxdepth: 2



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
