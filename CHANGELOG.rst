0.11.2 (2021-05-08)
-------------------

Misc
++++

- implemented ``GroupBy.filter``, which returns generator of results without
  creating an intermediate list

----


0.11.1 (2021-05-07)
-------------------

Bugfix
++++++

- fixed complex conversion cases where there are multiple aggregations
  `#27 <https://github.com/itechart/convtools/issues/27>`_

----


0.11.0 (2021-05-06)
-------------------

Features
++++++++

`#26 <https://github.com/itechart/convtools/issues/26>`_

- reimplemented pipes as a separate conversion + smart inlining
- now pipes are the only conversions which take care of adding labels
- introduced ``c.iter``: shortcut for ``self.pipe(c.generator_comp(element_conv))``
- introduced ``c.iter_mut``: generates the code which iterates and mutates the
  elements in-place. The result is a generator.

Bugfix
++++++

- fixed ``GroupBy.filter`` method to return generator by default, instead of
  list

----


0.10.0 (2021-04-28)
-------------------

Features
++++++++

`#25 by Anexen <https://github.com/itechart/convtools/issues/25>`_

- introduced ``c.ReduceFuncs.Average`` - arithmetic mean or weighted mean
- introduced ``c.ReduceFuncs.Median``
- introduced ``c.ReduceFuncs.Mode`` - most frequent value; last one if there are
  many of the same frequency
- introduced ``c.ReduceFuncs.TopK`` - list of most frequent values

----



0.9.4 (2021-04-27)
------------------

Bugfix
++++++

- fixed ``c.item(..., default=c.input_arg("abc"))``-like cases, where input
  args passed to item/attr with defaults

----


0.9.3 (2021-04-11)
------------------

Bugfix
++++++

- fixed ``c.group_by`` case without reducers like:
  ``c.group_by(c.item(0)).aggregate(c.item(0))``

----


0.9.2 (2021-03-28)
------------------

Misc
++++

- removed unnecessary ``debug=True`` enabled by default for ``join`` conversions

----


0.9.1 (2021-03-28)
------------------

Bugfix
++++++

`#24 <https://github.com/itechart/convtools/issues/24>`_

- fixed populating ``linecache`` with source code (previously new lines were not preserved) -- debugging issue

----

0.9.0 (2021-03-24)
------------------

Features
++++++++

`#23 <https://github.com/itechart/convtools/issues/23>`_


- improved reducers to be usable on their own

  .. code-block:: python

    c.aggregate(
        c.ReduceFuncs.DictSum(
            c.item("name"),
            c.item("value")
        )
    )

  previously it was possible to use them only within ``c.reduce`` clause:

  .. code-block:: python

    c.aggregate(
        c.reduce(
            c.ReduceFuncs.DictSum,
            (c.item("name"), c.item("value")),
        )
    )

- allowed piping to reducers, still allowing to pipe the result further

  .. code-block:: python

    c.aggregate(
        c.item("value").pipe(
            c.ReduceFuncs.Sum(c.this()).pipe(c.this() + 1)
        )
    ).gen_converter(debug=True)

- fixed nested piping in aggregations
- reworked docs to use testable code


----


0.8.0 (2021-01-03)
------------------

Misc
++++

- improved pylint rating
- added a few type hints
- added a few docstings


----


0.7.2 (2020-11-12)
------------------

Misc
++++

- `#22 <https://github.com/itechart/convtools/issues/22>`_


----


0.7.1 (2020-07-12)
------------------

Bugfixes
++++++++

- Fixed name generation uniqueness issue
  `#21 <https://github.com/itechart/convtools/issues/21>`_


----


0.7.0 (2020-06-14)
------------------

Features
++++++++

- Introduced ``c.Mut.set_item`` and other mutations to be used in ``(...).tap(...)``` method
  `#20 <https://github.com/itechart/convtools/issues/20>`_


----


0.6.1 (2020-05-18)
------------------

Bugfixes
++++++++

- fixed ``gen_name`` usages (made ``item_to_hash`` mandatory)
  `#19 <https://github.com/itechart/convtools/issues/19>`_


----


0.6.0 (2020-05-17)
------------------

Features
++++++++

- * introduced ``c.optional`` collection items, which get omitted based on value or a condition
  * improved converter generation so that inner conversions are not getting their own callable wrapper
  * updated generated code variable name generation `#18 <https://github.com/itechart/convtools/issues/18>`_


----


0.5.3 (2020-03-30)
------------------

Bugfixes
++++++++

- fixed aggregate issue: reduce(...).item(..., default=...) case `#15 <https://github.com/itechart/convtools/issues/15>`_


----


0.5.2 (2020-03-29)
------------------

Bugfixes
++++++++

- fixed Aggregate multiple reduce optimization
- added main page
- added workflow example

`#14 <https://github.com/itechart/convtools/issues/14>`_


----


0.5.1 (2020-03-26)
------------------

Updated index page docs.


----


0.5.0 (2020-03-23)
------------------

Features
++++++++

- - increased the speed of ``c.aggregate`` and ``c.group_by`` by collapsing multiple ``if`` statements into one
  - updated labeling functionality

  `#11 <https://github.com/itechart/convtools/issues/11>`_


----


0.4.0 (2020-03-19)
------------------

Features
++++++++

- Improved the way ``linecache`` is used: now the number of files to be put
  into the ``linecache`` is limited to 100. The eviction is done by implementing
  recently used strategy.
  `#9 <https://github.com/itechart/convtools/issues/9>`_
- - introduced ``c.join``
  - improved & fixed pipes (code with side-effects piped to a constant)

  `#10 <https://github.com/itechart/convtools/issues/10>`_


----


0.3.3 (2020-03-06)
------------------

Features
++++++++

- 1. fixed main example docs
  2. improved ``c.aggregate`` speed

  `#8 <https://github.com/itechart/convtools/issues/8>`_


----


0.3.2 (2020-03-05)
------------------

Improved Documentation
++++++++++++++++++++++

- * updated docs (fixed numbers) and updated pypi docs


----


0.3.1 (2020-03-05)
------------------

Features
++++++++

- * introduced c.OptionsCtx
  * improved tests - memory leaks
  * improved docs - added the index page example; added an example to QuickStart

  `#7 <https://github.com/itechart/convtools/issues/7>`_


----


0.3.0 (2020-03-01)
------------------

Features
++++++++

- Introduced `labeling`:

    * ``c.item("companies").add_label("first_company", c.item(0))`` labels the first
      company in the list as `first_company` and allows to use it as
      ``c.label("first_company")`` further in next and even nested conversions
  
    * ``(...).pipe`` now receives 2 new arguments: 

      * `label_input`, to put some labels on the pipe input data
      * `label_output` to put labels on the output data.

      Both can be either ``str`` (label name to put on) or ``dict`` (keys are label names
      and values are conversions to apply to the data before labeling)

  `#6 <https://github.com/itechart/convtools/issues/6>`_


Bugfixes
++++++++

- Added ``__name__`` attribute to ctx. Now internal code from the generated converter is sending to Sentry (not only file name).
  Also the generated converter became a callable object, not a function.

  `#5 <https://github.com/itechart/convtools/issues/5>`_


----


0.2.3 (2020-02-27)
------------------

Bugfixes
++++++++

- Fixed ``c.group_by((c.item("name"),)).aggregate((c.item("name"), c.reduce(...)))``.
  Previously it was compiling successfully, now it raises ``ConversionException`` on ``gen_converter``
  because there is no explicit mention of ``c.item("name")`` field in group by keys (only tuple).

  `#4 <https://github.com/itechart/convtools/issues/4>`_


----


0.2.2 (2020-02-25)
------------------

Bugfixes
++++++++

- fixed ``c.aggregate`` to return a single value for empty input

  `#3 <https://github.com/itechart/convtools/issues/3>`_


----


0.2.1 (2020-02-24)
------------------

Bugfixes
++++++++

- ``c.aggregate`` now returns a single value (previously the result was a list of one item)

  `#2 <https://github.com/itechart/convtools/issues/2>`_


----


0.2.0 (2020-02-23)
------------------

Features
++++++++

- added ``c.if_`` conversion and introduced QuickStart docs

  `#1 <https://github.com/itechart/convtools/issues/1>`_

