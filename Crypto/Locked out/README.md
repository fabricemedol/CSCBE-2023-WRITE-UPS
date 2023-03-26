The custom hashing algorithm has very low [preimage resistance](https://en.wikipedia.org/wiki/Preimage_attack). Given a single block input `target` (multiblock solution is similar), it is easy to construct a two-block input `constructed = block1 || block2` with the same hash. The hash of `target` is equal to `AES(target, salt)` and the hash of `constructed` is equal to `AES(block2 XOR AES(block1, salt), salt)`. It is easy to see that these two are the same if `block2 = AES(block1, salt) XOR target`. There is one more problem: `constructed` needs to consist of printable characters. There are 99 such characters (using newlines causes the password to end early), so the odds of `constructed` containing only printable characters is `99/256 ** 16`, which is about 1 in 4 million. This is bruteforceable in just a couple of minutes (1-2 minutes on my laptop). Simply guess a `block1` with only printable characters, calculate the corresponding `block2` and retry if it is not printable.