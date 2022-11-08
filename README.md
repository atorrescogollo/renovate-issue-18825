# renovate-issue-18825
https://github.com/renovatebot/renovate/issues/18825

Having this gradle dependencies:
```
// settings.gradle
...
            version("util.mapstruct", "1.5.2.Final")

            library("util.mapstruct", "org.mapstruct", "mapstruct").versionRef("util.mapstruct")
            library("util.mapstruct.processor", "org.mapstruct", "mapstruct-processor").versionRef("util.mapstruct")
...
```
The **expected result** would be like this:
```
Dependency Dashboard
------------------------
...
> Detected dependencies
gradle
    settings.gradle
       - `org.mapstruct:mapstruct 1.5.2.Final`
       - `org.mapstruct:mapstruct-processor 1.5.2.Final`
```
However, the **actual result** is:
```
Dependency Dashboard
------------------------
> Repository problems
      These problems occurred while renovating this repository.
      ```
      WARN: Error updating branch: update failure
      ```

> Errored
      These updates encountered an error and will be retried. Click on the checkbox below to force a retry now.

       - Update dependency org.mapstruct:mapstruct-processor to v1

> Detected dependencies
gradle
    settings.gradle
       - `org.mapstruct:mapstruct 1.5.2.Final`
       - `org.mapstruct:mapstruct-processor org.mapstruct` <--- This is wrong
```

#### Useful extra notes
It looks like the library entry somehow overwrites the version entry since it works if I change the order of the clauses to something like this:
```
            version("util.mapstruct", "1.5.2.Final")

            library("util.mapstruct.processor", "org.mapstruct", "mapstruct-processor").versionRef("util.mapstruct")
            library("util.mapstruct", "org.mapstruct", "mapstruct").versionRef("util.mapstruct")
```
