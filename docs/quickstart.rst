Quickstart
==========

Basic usage
-----------

Version comparison examples::

    >>> from versions import Version
    >>> v1 = Version.parse('1')
    >>> v2 = Version.parse('2')
    >>> v1 == v2
    False
    >>> v1 != v2
    True
    >>> v1 > v2
    False
    >>> v1 < v2
    True
    >>> v1 >= v2
    False
    >>> v1 <= v2
    True

:meth:`.Version.parse` expects a
:ref:`version expression <version-expressions>` string and 
returns a corresponding :class:`Version` object::

    >>> from versions import Version
    >>> v = Version.parse('1.2.0-dev+foo.bar')
    >>> v.major, v.minor, v.patch, v.prerelease, v.build_metadata
    (1, 2, 0, 'dev', set(['foo', 'bar']))

If it isn't a semantic version string, the parser tries to normalize it::

    >>> v = Version.parse('1')
    >>> v.major, v.minor, v.patch, v.prerelease, v.build_metadata
    (1, 0, 0, None, None)


Version constraint matching
---------------------------

versions also implements version constraint parsing and evaluation::

    >>> from versions import Constraint
    >>> Constraint.parse('>1').match('2')
    True
    >>> Constraint.parse('<2').match(Version.parse('1'))
    True

For conveniance, constraint matching can be tested using the ``in`` operator::

    >>> '1.5' in Constraint.parse('<2')
    True
    >>> Version(2) in Constraint.parse('!=2')
    False

Constraints can be merged using :class:`Constraints`::

    >>> from versions import Constraints
    >>> '1.0' in Constraints.parse('>1,<2')
    False
    >>> '1.5' in Constraints.parse('>1,<2')
    True
    >>> '2.0' in Constraints.parse('>1,<2')
    False
