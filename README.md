rnglib_go
=========

A Go random number generator especially useful for generating 
quasi-random strings, files, and directories.

This package contains two classes, **SimpleRNG** and **SystemRNG**.
Both of these subclass Go's rand package and so rand's functions 
can be called through either of rnglib's two subclasses using 
exactly the same syntax.

**SimpleRNG** is the faster of the two.  It uses the Mersenne
Twister and so is completely predictable with a very long period.
It is suitable where speed and predictability are both important.
You can be certain that if you provide the same seed, then
the sequence of numbers generated will be exactly the same.  This is
often important for debugging.  

**SimpleRNG** is also very fast.  It is approximately 30% faster 
than Go's rand package in our tests.

Although the Mersenne Twister is absolutely predictable, it also has 
extremely good statistical properties.  Tests using standard
packages such as [Diehard](http://en.wikipedia.org/wiki/Diehard_tests)
and [Dieharder](http://www.phy.duke.edu/~rgb/General/dieharder.php) rank
the Mersenned Twister very highly.  The Twister is quite widely used 
(for example, in the Python libraries) because of this.

**SystemRNG** is a secure random number generator, meaning that it is
extremely difficult or impossible to predict the next number 
generated in a sequence.  It is based on the system's /dev/urandom.
This relies upon entropy accumulated by the system; when 
this is insufficient it will revert to using a very secure 
programmable random number generator.  SystemRNG is about 35x
slower than SimpleRNG in our test runs.

For normal use in testing, SimpleRNG is preferred.  When there is
a requirement for greater security, as in the generation of passwords 
and cryptographic keys with reasonable strength, SystemRNG is recommended.  

In addition to the functions available from Go's random package,
rnglib also provides

    NextBoolean()
    NextByte(max=256)
    NextBytes(buffer)
    NextInt32(n uint32)
    NextInt64(n uint64)
    NextFloat32()
    NextFloat64()
    NextFileName(maxLen int)
    NextDataFile(dirName string, maxLen, minLen int)
    NextDataDir(pathToDir string, depth, width, maxLen, minLen int)

Given a buffer of arbitrary length, nextBytes() will fill it with random
bytes.

File names generated by `nextFileName()` are at least one character long.  
The first letter must be alphabetic (including the underscore) 
whereas other characters may also be numeric or either of dot and dash
("." and "-").

The function `nextDataFile()` creates a file with a random name in the
directory indicated.  The file length will vary from minLen (which 
defaults to zero) up to but excluding maxLen bytes.

The `nextDataDir()` function creates a directory structure that is
'depth' deep and 'width' wide, where the latter means that there 
will be that many files in each directory and subdirectory created
(where a file is either a data file or a directory). `depth` and
`width` must both be at least 1.  `maxLen` and `minLen` are used as in
`nextDataFile()`.

## On-line Documentation
More information on the **rnglib_go** project can be found 
[here](https://jddixon.github.io/rnglib_go)
