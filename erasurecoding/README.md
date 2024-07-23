# Test Vector Documentation

## Overview
This folder contains test vectors with 10 different lengths of original data, each accompanied by its corresponding codewords in JSON format. The data lengths are: 1, 32, 684, 4096, 4014, 15000, 21824, 21888, 100000, and 200000 bytes. The schema is defined in `vector_schema.json`, adhering to Draft 7 of the JSON Schema standard.

## Package Structure
Each `package_N` file includes original data of size $N$ bytes and the corresponding Reed-Solomon (RS) codewords encoded from it. For example, `package_684` contains raw data of 684 bytes and its encoded codewords.

## Data Format
- All data is encoded in HEX format, where each character represents 4 bits (a nibble).
- The original data is divided into segments of 4104 bytes each. The number of segments, $M$, is calculated as $M = \lceil N / 4014 \rceil$. The last segment is padded with zeros to ensure uniform size.
- Each segment of the original data is encoded with Reed-Solomon (RS) coding into a segment encoded codeword (`segment_ec`).
- Each `segment_ec` contains 1026 shards, and each shard contains $M$ byte pairs.
- Due to the nature of RS coding (BCH), the first 342 shards will be exactly the same as the original data with some byte-wise rearranging (see **Indexing**), while the remaining 684 shards are RS recovery shards.

**Indexing:** The $i$th RS codeword of the $j$th segment is stored in the $j$th byte pair of the $i$th shard. 

The term "byte pair" is used because Reed-Solomon coding is based on Galois Field $GF(2^{16})$, where each point in the field is 2 bytes long, making it the smallest unit of the RS codeword in this context.
