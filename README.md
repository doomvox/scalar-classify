# NAME

Scalar::Classify - get type and class information for scalars

# SYNOPSIS

     use Scalar::Classify qw( classify classify_pair );

     # determine the type (e.g. HASH for a hashref) and the object class (if any)
     my ( $type, $class ) = classify( $some_scalar );


    # warn if two args differ, supply default if one is undef
    my $default_value =
      classify_pair( $arg1, $arg2 );

    # Also get type and class; error out if two args differ
    my ( $default_value, $type, $class ) =
      classify_pair( $arg1, $arg2, { mismatch_policy => 'error' });

    # If a given ref was undef, replace it with a default value
    classify_pair( $arg1, $arg2, { also_qualify => 1 });

# DESCRIPTION

Scalar::Classify provides a routine named "classify" that can be used
to examine a given argument to determine it's type and class (if any).

Here "type" means either the return from reftype (, or if it's a scalar,
a code indicating whether it's a string or a number, and "class"
it the object class, the way a reference has been blessed.

This module also provides the routine "classify\_pair", which
looks at a pair of variables intended to be of the same type, and
if at least one of them is defined, uses that to get an
appropriate default value for that type.

## MOTIVATION

Perl contains a built-in "ref" function, and has some useful
routines in the standard Scalar::Util library ('ref',
'looks\_like\_number') which can be used to examine the type of an
argument.  The classify routine provided here internally uses all
three of these, returning a two-values that describe the kind of
thing you're examining.

The immediate goal was to provide support routines for the
[Data::Math](https://metacpan.org/pod/Data::Math) project.

## EXPORT

None by default. Optionally:

- classify

    Example usage:

        my ( $type, $class ) = classify( $some_var );

    Returns two pieces of information, the underlying "type", and the
    "class" (if this is a reference blessed into a class).

    The type is most often (but not limited to) one of the following:

        ARRAY
        HASH
        :NUMBER:
        :STRING:

    Other possibilities are the other potential returns from [ref](https://metacpan.org/pod/ref):

        CODE
        GLOB
        LVALUE
        FORMAT
        IO
        VSTRING
        Regexp

    Internally, this uses the built-in function [ref](https://metacpan.org/pod/ref) and the library
    functions [reftype](https://metacpan.org/pod/reftype) and [looks\_like\_number](https://metacpan.org/pod/looks_like_number) (from [Scalar::Util](https://metacpan.org/pod/Scalar::Util)).
    The type is the return from "reftype" (e.g "ARRAY", "HASH")
    except that in the case of a simple scalar the type is a code to
    indicate whether it seems to be a number (":NUMBER:") or a string
    (":STRING:").

    Note: if the argument is undefined, the returned type is undef.

- classify\_pair

    Examines a pair of arguments that are intended to be processed in
    parallel and are expected to be of the same type:

    If they're both defined, it checks that their types match.
    If at least one is defined, it generates a default of the
    same type by using the [classify](https://metacpan.org/pod/classify) method.  If both are
    undef, this default is also undef.

    In scalar context, it returns just the default value.

    In list context, it returns the default plus the type and
    the class (if it's a blessed reference).

    An options hashref is accepted as a third argument, with
    allowed options:

        o  mismatch_policy

           If argument types mismatch, the behavior is determined by
           the mismatch_policy option, defaulting to 'warn'.
           The other allowed values are 'error' or 'silent'.

        o  also_qualify

           If the "also_qualify" option is set to a true value, then
           the given arguments may be modified in place: if one is
           undef, it will be assigned the determined default.

    Examples:

        my $default_value =
          classify_pair( $arg1, $arg2, { mismatch_policy => 'error' });

        my ( $default_value, $type, $class ) =
          classify_pair( $arg1, $arg2, { mismatch_policy => 'error' });

        classify_pair( $arg1, $arg2, { also_qualify => 1 });

    Note the slightly unusual polymorphic behavior: in scalar
    context returns \*just\* the default\_value, in list context,
    returns up to three values, the default, the type and the class.

# SEE ALSO

[Params::Classify](https://metacpan.org/pod/Params::Classify)

This covers the argument checking case, where you want to verify
that something of the correct type was passed.  The perl5-porters
are interested in adding core support for this module: it's fast
and likely to get faster.

# AUTHOR

Joseph Brenner, <doom@kzsu.stanford.edu>

# COPYRIGHT AND LICENSE

Copyright (C) 2016 by Joseph Brenner

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

See http://dev.perl.org/licenses/ for more information.
