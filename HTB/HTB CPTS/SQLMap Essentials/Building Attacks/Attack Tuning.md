Every payload sent to the target consists of:
- vector: central part of payload (UNION ALL SELECT 1,2,VERSION())
- boundaries: prefix and suffix formations, used for proper injection of the vector into the vulnerable SQL statement ('<vector>-- -)

## Prefix/Suffix
**--prefix
**--suffix
