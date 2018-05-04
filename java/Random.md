# Random int generation

## Math.random()

```java
int min = 50;
int max = 100;
int rand = min + (int) (Math.random() * (max - min + 1));
```

Note that this approach is more biased and less efficient than a nextInt approach, https://stackoverflow.com/a/738651/360211

One standard pattern for accomplishing this is:

```java
Min + (int)(Math.random() * ((Max - Min) + 1))
```

The Java Math library function Math.random() generates a double value in the range [0,1). Notice this range does not include the 1.

In order to get a specific range of values first, you need to multiply by the magnitude of the range of values you want covered.

```java
Math.random() * ( Max - Min )
```

This returns a value in the range [0,Max-Min), where 'Max-Min' is not included.

For example, if you want [5,10], you need to cover five integer values so you use

```java
Math.random() * 5
```

This would return a value in the range [0,5), where 5 is not included.

Now you need to shift this range up to the range that you are targeting. You do this by adding the Min value.

```java
Min + (Math.random() * (Max - Min))
```

You now will get a value in the range [Min,Max). Following our example, that means [5,10):

```java
5 + (Math.random() * (10 - 5))
```

But, this still doesn't include Max and you are getting a double value. In order to get the Max value included, you need to add 1 to your range parameter (Max - Min) and then truncate the decimal part by casting to an int. This is accomplished via:

```java
Min + (int)(Math.random() * ((Max - Min) + 1))
```

And there you have it. A random integer value in the range [Min,Max], or per the example [5,10]:

```java
5 + (int)(Math.random() * ((10 - 5) + 1))
```

## Random.nextInt(n)

Here is the detailed explanation of why "Random.nextInt(n) is both more efficient and less biased than Math.random() * n" from the Sun forums post that Gili linked to:

Math.random() uses Random.nextDouble() internally.

Random.nextDouble() uses Random.next() twice to generate a double that has approximately uniformly distributed bits in its mantissa, so it is uniformly distributed in the range 0 to 1-(2^-53).

Random.nextInt(n) uses Random.next() less than twice on average- it uses it once, and if the value obtained is above the highest multiple of n below MAX_INT it tries again, otherwise is returns the value modulo n (this prevents the values above the highest multiple of n below MAX_INT skewing the distribution), so returning a value which is uniformly distributed in the range 0 to n-1.

Prior to scaling by 6, the output of Math.random() is one of 2^53 possible values drawn from a uniform distribution.

Scaling by 6 doesn't alter the number of possible values, and casting to an int then forces these values into one of six 'buckets' (0, 1, 2, 3, 4, 5), each bucket corresponding to ranges encompassing either 1501199875790165 or 1501199875790166 of the possible values (as 6 is not a disvisor of 2^53). This means that for a sufficient number of dice rolls (or a die with a sufficiently large number of sides), the die will show itself to be biased towards the larger buckets.

You will be waiting a very long time rolling dice for this effect to show up.

Math.random() also requires about twice the processing and is subject to synchronization.