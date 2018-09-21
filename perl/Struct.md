# Struct

```perl
use strict;
use warnings;
use Class::Struct;

        # declare struct, based on array:
struct( CLASS_NAME => [ ELEMENT_NAME => ELEMENT_TYPE, ... ]);

        # declare struct, based on hash:
struct( CLASS_NAME => { ELEMENT_NAME => ELEMENT_TYPE, ... });

package CLASS_NAME;
use Class::Struct;
        # declare struct, based on array, implicit class name:
struct( ELEMENT_NAME => ELEMENT_TYPE, ... );
# Declare struct at compile time
use Class::Struct CLASS_NAME => [ELEMENT_NAME => ELEMENT_TYPE, ...];
use Class::Struct CLASS_NAME => {ELEMENT_NAME => ELEMENT_TYPE, ...};
# declare struct at compile time, based on array, implicit
# class name:
package CLASS_NAME;
use Class::Struct ELEMENT_NAME => ELEMENT_TYPE, ... ;
package Myobj;
use Class::Struct;
        # declare struct with four types of elements:
struct( s => '$', a => '@', h => '%', c => 'My_Other_Class' );
$obj = new Myobj;               # constructor
                                # scalar type accessor:
$element_value = $obj->s;           # element value
$obj->s('new value');               # assign to element
                                # array type accessor:
$ary_ref = $obj->a;                 # reference to whole array
$ary_element_value = $obj->a(2);    # array element value
$obj->a(2, 'new value');            # assign to array element
                                # hash type accessor:
$hash_ref = $obj->h;                # reference to whole hash
$hash_element_value = $obj->h('x'); # hash element value
$obj->h('x', 'new value');          # assign to hash element
                                # class type accessor:
$element_value = $obj->c;           # object reference
$obj->c->method(...);               # call method of object
$obj->c(new My_Other_Class);        # assign a new object
```


```perl
use Class::Struct;

struct( Rusage => {
    ru_utime => 'Timeval',  # user time used
    ru_stime => 'Timeval',  # system time used
});
struct( Timeval => [
    tv_secs  => '$',        # seconds
    tv_usecs => '$',        # microseconds
]);
# create an object:
my $t = Rusage->new(ru_utime=>Timeval->new(),
    ru_stime=>Timeval->new());
# $t->ru_utime and $t->ru_stime are objects of type Timeval.
# set $t->ru_utime to 100.0 sec and $t->ru_stime to 5.0 sec.
$t->ru_utime->tv_secs(100);
$t->ru_utime->tv_usecs(0);
$t->ru_stime->tv_secs(5);
$t->ru_stime->tv_usecs(0);

```