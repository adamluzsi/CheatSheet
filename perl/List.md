# List

## Syntax

```perl
my @list;
```

## Push

```perl
push(@list, $element);
```

## Pop / Shift

```perl
my @names = ('Foo', 'Bar', 'Moo');
my $first = shift @names;
print "$first\n";     # Foo
print "@names\n";     # Bar Moo
```

## Unshift

```perl
my @names = ('Foo', 'Bar');
unshift @names, 'Moo';
print "@names\n";     # Moo Foo Bar

my @others = ('Darth', 'Vader');
unshift @names, @others;
print "@names\n";     # Darth Vader Moo Foo Bar
```

## Uniq

```perl
my @names = ("Foo", "Bar", "Baz", "Foo");
my @names = do { my %seen; grep { !$seen{$_}++ } @names };
print @names;
```
