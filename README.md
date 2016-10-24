# NAME

Scalar::Classify - given a scalar, examine it's type and class

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

This module also provides the routine "classify_pair", which
looks at a pair of variables intended to be of the same type, and
if at least one of them is defined, uses that to get an
appropriate default value for that type.

# AUTHOR

Joseph Brenner <doom@kzsu.stanford.edu>

# COPYRIGHT

Copyright (C) 2016 by Joseph Brenner

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
