# ngt-rs &emsp; [![Latest Version]][crates.io] [![Latest Doc]][docs.rs]

[Latest Version]: https://img.shields.io/crates/v/ngt.svg
[crates.io]: https://crates.io/crates/ngt
[Latest Doc]: https://docs.rs/ngt/badge.svg
[docs.rs]: https://docs.rs/ngt

Rust wrappers for [NGT][], which provides high-speed approximate nearest neighbor
searches against a large volume of data.

Building NGT requires `CMake`. By default `ngt-rs` will be built dynamically, which
means that you'll need to make the build artifact `libngt.so` available to your final
binary. You'll also need to have `OpenMP` installed on the system where it will run. If
you want to build `ngt-rs` statically, then use the `static` Cargo feature, note that in
this case `OpenMP` will be disabled when building NGT.

Furthermore, NGT's shared memory and large dataset features are available through Cargo
features `shared_mem` and `large_data` respectively.

## Usage

Defining the properties of a new index:

```rust
use ngt::{Properties, DistanceType, ObjectType};

// Defaut properties with vectors of dimension 3
let prop = Properties::dimension(3)?;

// Or customize values (here are the defaults)
let prop = Properties::dimension(3)?
    .creation_edge_size(10)?
    .search_edge_size(40)?
    .object_type(ObjectType::Float)?
    .distance_type(DistanceType::L2)?;
```

Creating/Opening an index and using it:

```rust
use ngt::{Index, Properties, EPSILON};

// Create a new index
let prop = Properties::dimension(3)?;
let index = Index::create("target/path/to/index/dir", prop)?;

// Open an existing index
let mut index = Index::open("target/path/to/index/dir")?;

// Insert two vectors and get their id
let vec1 = vec![1.0, 2.0, 3.0];
let vec2 = vec![4.0, 5.0, 6.0];
let id1 = index.insert(vec1)?;
let id2 = index.insert(vec2)?;

// Actually build the index (not yet persisted on disk)
// This is required in order to be able to search vectors
index.build(2)?;

// Perform a vector search (with 1 result)
let res = index.search(&vec![1.1, 2.1, 3.1], 1, EPSILON)?;
assert_eq!(res[0].id, id1);
assert_eq!(index.get_vec(id1)?, vec![1.0, 2.0, 3.0]);

// Remove a vector and check that it is not present anymore
index.remove(id1)?;
let res = index.get_vec(id1);
assert!(matches!(res, Result::Err(_)));

// Verify that now our search result is different
let res = index.search(&vec![1.1, 2.1, 3.1], 1, EPSILON)?;
assert_eq!(res[0].id, id2);
assert_eq!(index.get_vec(id2)?, vec![4.0, 5.0, 6.0]);

// Persist index on disk
index.persist()?;
```

[ngt]: https://github.com/yahoojapan/NGT
