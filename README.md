# Oyente

**Note: The tool is currently under development, please report any bugs you may find.**

## Quick Start

A container with the dependencies set up and the blockchain snapshot installed can be found [here](https://hub.docker.com/r/hrishioa/oyente/).

To open the container, install docker and run:

```docker pull hrishioa/oyente && docker run -i -t hrishioa/oyente```

To evaluate the greeter contract Inside the container, run:

```cd /home/oyente/oyente && source ../dependencies/venv/bin/activate && python oyente.py greeter.sol```

and you are done!

## Benchmarks

To run the benchmarks, the fastest way is to use the docker image.

The blockchain snapshot is in `contract_data` and the benchmark results are in `benchmarks/results.json`. The benchmarks were run using a 5-minute timeout - using a longer timeout will give fewer incomplete executions.

In the container, run `batch_run.py` after activating the virtualenv. Results are in `results.json` once the benchmark completes.

The benchmarks take a long time and a *lot* of RAM in any but the largest of clusters, beware.

### Benchmark Results

| Bug             | Occurrences |
| --------------- | :---------- |
| Callstack Bug   | 4792        |
| Reentrancy      | 3742        |
| Time Dependancy | 165         |
| Concurrency     | 490         |
| Total           | 11839       |
| (Timeout)       | 5715        |

*The results are different from those in the paper. This is an improved version of the tool, with fewer exceptions.* 

## Dependencies

1. solc and disasm from [go-ethereum](https://github.com/ethereum/go-ethereum)
2. [z3](https://github.com/Z3Prover/z3/releases) Theorem Prover

## Evaluating Ethereum Contracts

```python oyente.py <contract filename>```

And that's it! Run ```python oyente.py --help``` for a list of options.

## Paper

The accompanying paper explaining the bugs detected by the tool can be found [here](http://www.comp.nus.edu.sg/~loiluu/papers/oyente.pdf).

## Miscellaneous Utilities

A collection of the utilities that were developed for the paper are in `Misc_Utils`. Use them at your own risk - they have mostly been disposable.

1. `generate-graphs.py` - Contains a number of functions to get statistics from contracts.
2. `get_source.py` - The *get_contract_code* function can be used to retrieve contract source from [EtherScan](https://etherscan.io)
3. `transaction_scrape.py` - Contains functions to retrieve up-to-date transaction information for a particular contract.

#### Known Issues
If you encounter the `unhashable instance` error, please add the following to your `class AstRef(Z3PPObject):` in `/usr/lib/python2.7/dist-packages/z3.py`:
```
def __hash__(self):
        return self.hash()
```
The latest version of Z3 does support this, but some previous version does not.
