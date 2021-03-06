
Allow to inject warning filters during `nosetest`.


Put the same arguments as `warnings.filterwarnings` in `setup.cfg` at the root of your project.
Separated each argument by pipes `|`, one filter per line.
Whitespace are stripped.

for example:
```
[nosetests]
warningfilters=default         |.*            |DeprecationWarning |notebook.*
               ignore          |.*metadata.*  |DeprecationWarning |notebook.*
               once            |.*schema.*    |UserWarning        |nbfor.*
               error           |.*warn.*      |DeprecationWarning |notebook.services.contents.manager*
```

If you prefer another name for the configuration file, you can tell nose to load
the configuration using the `-c` flag: run the tests with `nosetests -c nose.cfg`.


# details configuration. 

Each line of warning filter is separated in maximum 4 sections, that match the first 4 sections of `filterwarnings`:

```python
filterwarnings(action, message="", category=Warning, module="", lineno=0, append=False)
```
fields 2 to 4 can be omitted, ie to say 1 line can be of the following form:

```
action
action| message
action| message | category
action| message | category | module
```

the value of each fields is treated the same as for `filterwarnigns` except:
    - whitespace are trimmed. 
    - if the `category` has dots, the corresponding class try to be imported.
      If it does not have dots, the name is looked up in `builtins` or
      `__builtins__`



# test are failing

For some reasons in some systems tests are failing; it seem that this package
have difficulty to self-test. That's likely due to the fact that the tested
package need to be in different namespaces, and by self-testing we break this
assumption. 


