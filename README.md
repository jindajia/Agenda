
# Single Source Personalized PageRank

## Tested Environment
- Ubuntu
- C++ 11
- GCC 4.8
- Boost
- cmake

## Compile
```sh
$ cmake .
$ make
```

## Parameters
```sh
./fora action_name --algo <algorithm> [options]
```
- action:
    - query: static SSPPR query
    - topk: static top-k query
    - build: build index, for FORA only
    - dynamic-ss
    - dynamic-topk
    - dynamic-onehop
    - dynamic-hybird
- algo: which algorithm you prefer to run
    - baton: Baton
    - fora: FORA
    - partup: Genda with partial update
    - lazyup: Genda with lazy update
    - resacc: ResAcc
- options
    - --prefix \<prefix\>
    - --epsilon \<epsilon\>
    - --dataset \<dataset\>
    - --query_size \<queries count\>
    - --k \<top k\>
    - --with_idx
    - --beta
    - --baton


## Data
The example data format is in `./data/webstanford/` folder. The data for DBLP, Pokec, Orkut, LiveJournal, Twitter are not included here for size limitation reason. You can find them online.

## Generate workloads
Generate query files for the graph data. Each line contains a node id.

```sh
$ ./fora generate-ss-query --prefix <data-folder> --dataset <graph-name> --query_size <query count>
```

```sh
$ ./fora gen-update --prefix <data-folder> --dataset <graph-name> --query_size <query count>
```

- Example:

```sh
$ ./fora generate-ss-query --prefix ./data/ --dataset webstanford --query_size 1000
```

## Indexing
Construct index files for the graph data using a single core.

Note: the code of indexing for hubppr is not included here, please turn to the code of hubppr: https://sourceforge.net/projects/hubppr/

- Example

For FORA index:
```sh
$ ./fora build --prefix ./data/ --dataset webstanford --epsilon 0.5
```
For BATON index:
```sh
$ ./fora build --prefix ./data/ --dataset webstanford --epsilon 0.5 --baton
```

## Query
Process queries.

```sh
$ ./fora <query-type> --algo <algo-name> --prefix <data-folder> --dataset <graph-name> --result_dir <output-folder> --epsilon <relative error> --query_size <query count> --update_size<update count> [--with-idx --exact]
```

- Example:

For static queries

```sh
// FORA without index KDD version
$ ./fora query --algo fora --prefix ./data/ --dataset webstanford --epsilon 0.5 --query_size 20

// FORA without index TODS version (balance strategy and optimization technique included)
$ ./fora query --algo fora --prefix ./data/ --dataset webstanford --epsilon 0.5 --query_size 20 --balanced --opt

// FORA with index KDD version
$ ./fora query --algo fora --prefix ./data/ --dataset webstanford --epsilon 0.5 --query_size 20 --with_idx

// FORA with index TODS version
$ ./fora query --algo fora --prefix ./data/ --dataset webstanford --epsilon 0.5 --query_size 20 --with_idx --opt

// Genda 
$ ./fora query --algo genda --prefix ./data/ --dataset webstanford --epsilon 0.5 --query_size 20 --with_idx
```

For dynamic SSPPR:
```sh
$ ./fora dynamic-ss --algo lazyup --epsilon 0.5 --prefix ./data/ --dataset webstanford --query_size 200 --update_size 200 --beta 1 --with_idx
```

For dynamic top-k PPR:
```sh
$ ./fora dynamic-topk --algo lazyup --epsilon 0.5 --prefix ./data/ --dataset webstanford --query_size 200 --update_size 200 --beta 1 --with_idx
```

For dynamic onehop PPR:
```sh
$ ./fora dynamic-onehop --algo lazyup --epsilon 0.5 --prefix ./data/ --dataset webstanford --query_size 200 --update_size 200 --beta 1 --with_idx
```


