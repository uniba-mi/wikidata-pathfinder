# Wikidata Pathfinder

<p align="center">
    <img src="https://img.shields.io/badge/license-GPLv3-green.svg" alt="license">
    <br>
</p>

<p align="center">
    <a href="#requirements">Requirements</a>
    •
    <a href="#query-factory">Query Factory</a>
    •
    <a href="#dual-entity-query-dataset">Dual-Entity Query Dataset</a>
    •
    <a href="#pathfinder-implementation">Pathfinder Implementation</a>
    •
    <a href="#license">License</a>
</p>

This repository contains all relevant materials that are necessary to reproduce the outcomes described in the research paper _Further Investigation of Fast Pathfinding in Wikidata_. This comprises the following artifacts:

 - A Query Factory for deriving a dual-entity query dataset for pathfinding in Wikidata
 - The derived dual-entity query dataset
 - A Pathfinding System for finding paths between the entities in Wikidata

 The following paragraphs provide additional information about each artifact. This includes instructions for reproducing the results mentioned in the paper.

## Requirements

Only the `docker` and `docker-compose` scripts are required to run the programs within this repository. All dependencies are automatically installed using the corresponding Dockerfiles. This ensures reproducibility and ease of use. For guidance on how to install Docker click [here](https://docs.docker.com/get-docker/).

## Query Factory

The purpose of the Query Factory is to derive dual-entity queries for pathfinding in Wikidata from the [TREC 2007 Million Queries Track dataset](http://trec.nist.gov/data/million.query07.html). For identifying and disambiguating the entities mentioned in the TREC queries the [GENRE entity linker](https://github.com/facebookresearch/GENRE) is employed.

### Usage

To run the Query Factory proceed as follows:

1. Select the TREC file from which queries should be derived by adjusting the commented parts in the [query_factory.py](./src/query_factory.py). 
2. Run ``docker compose run query_factory`` from the root directory.
3. In the new bash run ``factory 07`` to start the query factory. Warning: This will overwrite the already present [dual-entity query dataset](./data/wikidata_queries_10000_topics_genre.csv).

## Dual-Entity Query Dataset

The derived dual-entity query dataset can be found [here](./data/wikidata_queries_10000_topics_genre.csv). It uses the CSV format; the columns have the following meaning:

- wikidata_id_a: The Wikidata ID of the first entity of the query
- wikidata_id_b: The Wikidata ID of the second entity of the query
- trec_id: The ID of the original TREC query

## Pathfinding System

This artifact actually comprises three components that implement the pathfinding. The Pathfinder component contains the actual pathfinding algorithm and interacts with two API over HTTP: To issue queries on Wikidata, it interacts with the Wikidata API component and, to calculate the semantic distances between entities, it interacts with the Wembed API.

### Usage

To run the Pathfinding System proceed as follows:

1. Launch the Wikidata API via `docker-compose run --service-ports wikidata_api` in a separate bash.
2. Launch the Wembed API via `docker-compose run --service-ports wembed_api` in a separate bash.
3. Run `docker-compose run pathfinder` in a separate bash to launch the Pathfinder component. There are several commands that can be used in this new bash:
    1. Run `cargo run -- playground` to launch the pathfinder on a few example queries.
    2. Run `cargo run -- optimizer` to run the optimizer for fitting the search parameters alpha, beta, and gamma. Warning: This will overwrite the already present [optimizer results file](./data/optimizer_results.csv).
    3. Run `cargo run -- benchmark` to run the benchmark. Warning: This will overwrite the already present [benchmark results files](./data/).

## License

See [LICENSE](./LICENSE)
