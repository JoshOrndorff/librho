# A Standard Library for Rholang

This project is in its infancy. It aspires to be a standard library for [rholang](https://rchain.coop) akin to libc. At the moment it is a few instructive and semi-useful algorithms and mathematical features implemented in pure rholang.

The coding and testing standards for this library (and the entire language) are still evolving. The [librho style guide](styleguide.md) is a step. Testing standards remain to be developed.

## Unsafe Math
Basic mathematical operations such as exponentiation and modular division to operate on rholang's built-in 64-bit integers. They make no attempt to avoid over- or underflows which is why they are unsafe.

## Safe Math
Aspirational: Things like big integers, rational numbers, and fixed (or maybe even floating) points.

## Object Capabilities
Aspirational: Reference implementations of the classic object capability patterns such as forwarders and seal/unsealers.

## List Operations
Search, Sort, Fold, Map, etc

## Classic Algorithms
Search, Sort, Pathfinding, etc

## Blockchain Patterns
Best practices for common blockchain operations. Again, any code here is in its infancy and is not formally verified. Use at your own risk.

## Multiple Arguments
Comparing and converiting three patterns for calling contracts with multiple arguments.
1. Tupling
2. Currying (not recommended here)
3. Multiplexing

## License
Apache 2.0
