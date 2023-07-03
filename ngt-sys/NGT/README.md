<div align="center">
<img src="./assets/logo.svg" width="50%">
</div>

Neighborhood Graph and Tree for Indexing High-dimensional Data

[Home](/README.md) / [Installation](/README.md#Installation) / [Command](/bin/ngt/README.md#command) / [License](/README.md#license) / [Publications](/README.md#publications) / [About Us](http://research-lab.yahoo.co.jp/en/) / [日本語](/README-jp.md)

**NGT** provides commands and a library for performing high-speed approximate nearest neighbor searches against a large volume of data (several million to several 10 million items of data) in high dimensional vector data space (several ten to several thousand dimensions).

News
----
- 02/04/2022 FP16 (half-precision floating point) is now available. (v1.14.0)
- 03/12/2021 The results for the quantized graph are added to this README.
- 01/15/2021 NGT v1.13.0 to provide the [quantized graph (NGTQG)](bin/ngtqg/README.md) is released.
- 11/04/2019 [NGT tutorial](https://github.com/yahoojapan/NGT/wiki) has been released.
- 06/26/2019 Jaccard distance is available. (v1.7.6)
- 06/10/2019 PyPI NGT package v1.7.5 is now available.
- 01/17/2019 Python NGT can be installed via pip from PyPI. (v1.5.1)
- 12/14/2018 [NGTQ](bin/ngtq/README.md) (NGT with Quantization) is now available. (v1.5.0)
- 08/08/2018 [ONNG](README.md#onng) is now available. (v1.4.0)

Key Features
------------
- Supported operating systems: Linux and macOS
- Object additional registration and removal are available.
- Objects beyond the memory size can be handled using [the shared memory (memory mapped file) option](README.md#shared-memory-use).
- Supported distance functions: L1, L2, Cosine similarity, Angular, Hamming, Jaccard, Poincare, and Lorentz
- Data Types: 4 byte floating point number and 1 byte unsigned integer
- Supported languages: [Python](/python/README.md), [Ruby](https://github.com/ankane/ngt), [Rust](https://crates.io/crates/ngt), [Go](https://github.com/yahoojapan/gongt), C, and C++
- Distributed servers: [ngtd](https://github.com/yahoojapan/ngtd) and [vald](https://github.com/vdaas/vald)
- [NGTQ](bin/ngtq/README.md) can handle billions of objects.

Documents
---------

- [NGT tutorial](https://github.com/yahoojapan/NGT/wiki)

Installation
------------

### Downloads

- [Releases](https://github.com/yahoojapan/NGT/releases)

### Pre-Built

#### On macOS

      $ brew install ngt

### Build

#### On Linux

      $ unzip NGT-x.x.x.zip
      $ cd NGT-x.x.x
      $ mkdir build
      $ cd build
      $ cmake ..
      $ make
      $ make install
      $ ldconfig /usr/local/lib

#### On macOS using homebrew

      $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      $ brew install cmake
      $ brew install gcc@9
      $ export CXX=/usr/local/bin/g++-9
      $ export CC=/usr/local/bin/gcc-9
      $ unzip NGT-x.x.x.zip
      $ cd NGT-x.x.x
      $ mkdir build
      $ cd build
      $ cmake ..
      $ make
      $ make install

#### Shared memory use

The index can be placed in shared memory with memory mapped files. Using shared memory can reduce the amount of memory needed when multiple processes are using the same index. In addition, it can not only handle an index with a large number of objects that cannot be loaded into memory, but also reduce time to open it. Since changes become necessary at build time, please add the following parameter when executing "cmake" in order to use shared memory.

      $ cmake -DNGT_SHARED_MEMORY_ALLOCATOR=ON ..

Note: Since there is no lock function, the index should be used only for reference when multiple processes are using the same index.

#### Large-scale data use

When you insert more than about 5 million objects, please add the following parameter to improve the search time.

      $ cmake -DNGT_LARGE_DATASET=ON ..

Utilities
---------

- Command : [ngt](/bin/ngt/README.md#command), [ngtq](bin/ngtq/README.md), [ngtqg](bin/ngtqg/README.md)
- Server : [ngtd](https://github.com/yahoojapan/ngtd), [vald](https://github.com/vdaas/vald)

Supported Programming Languages
-------------------------------

- [Python](/python/README.md)
- [Ruby](https://github.com/ankane/ngt) (Thanks Andrew!)
- [Rust](https://crates.io/crates/ngt) (Thanks Romain!)
- JavaScript/NodeJS : [ngt-tool](https://www.npmjs.com/package/ngt-tool), [spatial-db-ngt](https://www.npmjs.com/package/spatial-db-ngt) (Thanks stonkpunk!)
- [Go](https://github.com/yahoojapan/gongt)
- C
- C++([sample code](samples))

Benchmark Results
-----------------
The followings are the results of [ann benchmarks](https://github.com/erikbern/ann-benchmarks) for NGT v1.13.5 where the timeout is 5 hours on an AWS c5.4xlarge instance.

#### glove-100-angular
<img src="./tests/ann-benchmarks-results/glove-100-angular.png?raw=true" width="400">

#### gist-960-euclidean
<img src="./tests/ann-benchmarks-results/gist-960-euclidean.png?raw=true" width="400">

#### fashion-mnist-784-euclidean
<img src="./tests/ann-benchmarks-results/fashion-mnist-784-euclidean.png?raw=true" width="400">

#### nytimes-256-angular
<img src="./tests/ann-benchmarks-results/nytimes-256-angular.png?raw=true" width="400">

#### sift-128-euclidean
<img src="./tests/ann-benchmarks-results/sift-128-euclidean.png?raw=true" width="400">

License
-------

Copyright (C) 2015 Yahoo Japan Corporation

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this software except in compliance with the License. You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and limitations under the License.

Contributor License Agreement
-----------------------------

This project requires contributors to accept the terms in the [Contributor License Agreement (CLA)](https://gist.github.com/yahoojapanoss/9bf8afd6ea67f32d29b4082abf220340).

Please note that contributors to the NGT repository on GitHub (https://github.com/yahoojapan/NGT) shall be deemed to have accepted the CLA without individual written agreements.

Contact Person
--------------
[masajiro](https://github.com/masajiro)

Publications
------------
##### [ONNG](bin/ngt/README.md#onng)
- Iwasaki, M., Miyazaki, D.: Optimization of Indexing Based on k-Nearest Neighbor Graph for Proximity. arXiv:1810.07355 [cs] (2018). ([pdf](https://arxiv.org/abs/1810.07355))

##### [PANNG](bin/ngt/README.md#panng)
- Iwasaki, M.: Pruned Bi-directed K-nearest Neighbor Graph for Proximity Search. Proc. of SISAP2016 (2016) 20-33. ([pdf](https://link.springer.com/chapter/10.1007/978-3-319-46759-7_2))
- Sugawara, K., Kobayashi, H. and Iwasaki, M.: On Approximately Searching for Similar Word Embeddings. Proc. of ACL2016 (2016) 2265-2275. ([pdf](https://aclweb.org/anthology/P/P16/P16-1214.pdf))

##### [ANNGT](bin/ngt/README.md#anngt)
- Iwasaki, M.: Applying a Graph-Structured Index to Product Image Search (in Japanese). IIEEJ Journal 42(5) (2013) 633-641. ([pdf](https://s.yimg.jp/i/docs/research_lab/articles/miwasaki-iieej-jnl-2013.pdf))
- Iwasaki, M.: Proximity search using approximate k nearest neighbor graph with a tree structured index (in Japanese). IPSJ Journal 52(2) (2011) 817-828. ([pdf](https://s.yimg.jp/i/docs/research_lab/articles/miwasaki-ipsj-jnl-2011.pdf))

##### [ANNG](bin/ngt/README.md#anng)
- Iwasaki, M.: Proximity search in metric spaces using approximate k nearest neighbor graph (in Japanese). IPSJ Trans. on Database 3(1) (2010) 18-28. ([pdf](https://s.yimg.jp/i/docs/research_lab/articles/miwasaki-ipsj-tod-2010.pdf))


