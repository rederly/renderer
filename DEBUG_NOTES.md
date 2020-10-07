# Changes made for debugger
* Added launch.json
  * Added a space to the url references which is the last argument
    * zsh has errors if the url arg is not in quotes
    * if you add quotes vscode escapes them escapes them
* Started the update process on WWSafe.pm
* Using vscode plugin: [Perl Debug](https://marketplace.visualstudio.com/items?itemName=mortenhenriksen.perl-debug)
# The problem
Running with debugger causes compilations errors

## Command
```
perl -d `which morbo` ./script/render_app
```

## Error message
```
Can't load application from file "/home/luigi/rederly/source/renderer/renderer/script/render_app": Could not evaluate global environment file /home/luigi/rederly/source/renderer/renderer/lib/WeBWorK/conf/defaults.config: Undefined subroutine &Safe::Root0::include called at /usr/share/perl/5.30/perl5db.pl line 4216.
 at /home/luigi/rederly/source/renderer/renderer/lib/PG/lib/WeBWorK/PG/IO.pm line 20.
Compilation failed in require at /home/luigi/rederly/source/renderer/renderer/lib/PG/lib/WeBWorK/PG/Translator.pm line 16.
 at /home/luigi/rederly/source/renderer/renderer/lib/PG/lib/WeBWorK/PG/Translator.pm line 16.

 ...

```

# Safe
* WebWork made slight modifications to the safe module
* We suspect the environment errors are due to safe
* Safe needs to be updated regardless
* Commenting out both includes in default.conf results in course environment error instead

## Modifications
* Added `use utf8;`
* Switched name to `WWSafe` which appears once for the package name and once in documentation
* Changed the perl version to `use 5.12.0;` (both old version and new version `use 5.003_11;`)
* Removed:
  * $version::VERSION
  * $version::CLASS
  * @version::ISA

## New error
```
Can't load application from file "/home/luigi/rederly/source/renderer/renderer/script/render_app": Bad Safe object at /home/luigi/rederly/source/renderer/renderer/lib/WeBWorK/lib/WWSafe.pm line 356.

...

```

## Source
### Current version
https://metacpan.org/source/RGARCIA/Safe-2.16/Safe.pm
### Used version
https://metacpan.org/source/RGARCIA/Safe-2.35/Safe.pm
### Latest version
https://metacpan.org/release/Safe/source/Safe.pm
