# minbpe.🔥

This project is a port of Andrej Karpathy's [minbpe](https://github.com/karpathy/minbpe) to [Mojo](https://docs.modular.com/mojo), currently in alpha and actively developed. Not all features of `minpe` are available yet, but they will be introduced as the project evolves.

## Implementation

Due to differences in language capabilities, the architecture of this port has been modified to fit the constraints and features of Mojo. While the architecture is different, the core functionalities and behaviors of the application remain the same as in the original. As Mojo's language features continue to evolve, we expect to further refine and redesign the project.

## Available Tokenizer

Tokenizers in `minbpe.mojo` are implemented by confirming to the `TokenizationStrategy` trait, which defines the required methods around tokenization processes.

- **BasicTokenizationStrategy**: Implements the BasicTokenizer, the simplest implementation of the BPE algorithm that runs directly on text.
- **RegexTokenizationStrategy**: Implements the RegexTokenizer that further splits the input text by a regex pattern, which is a preprocessing stage that splits up the input text by categories.
- **GPT4TokenizationStrategy** coming soon

## Quick Start

- First make sure you have [Mojo 24.3](https://docs.modular.com/mojo/manual/get-started/) installed.  
- In addtion you need to install the Python library `regex`. We rely on `regex` because Mojo currently lacks a powerful native regular expression library. Mojo's ability to utilize Python libraries allows us to enhance functionality in this way. For information on this powerful language feature, see the [Python Integration](https://docs.modular.com/mojo/manual/python/) section in the official Mojo documentation.

 ```bash
 pip install regex
 ```

- The [quick start](https://github.com/karpathy/minbpe?tab=readme-ov-file#quick-start) example from `minbpe` can be implement with `minbpe.mojo` as follows:

 ```python
from mojobpe import Tokenizer,BasicTokenizationStrategy
from mojobpe.utils.tat import print_list_int

fn main() raises:
    var text = "aaabdaaabac"

    var tokenizer = Tokenizer[BasicTokenizationStrategy]()
    tokenizer.train(text, 256 + 3) # 256 are the byte tokens, then do 3 merges
    print_list_int(tokenizer.encode(text))
    # [258, 100, 258, 97, 99]

    print(tokenizer.decode(List[Int](258, 100, 258, 97, 99)))
    # aaabdaaabac

    tokenizer.save("toy")
    # writes toy.model (for loading) 
```

## Benchmarks

A detailed benchmark analysis will be available soon.

For now we have included a Mojo port of `train.py` from the original repository, which times the training of both the Basic and Regex Tokenizer with the text from Taylor Swift's Wikipedia page. In our preliminary tests, the Mojo version proves to be approximately three times faster than the original Python implementation. You can run this training benchmark test using the following command:

```bash
mojo train.mojo
```

### Remarks

- We achieved a significant performance boost by utilizing [Maxim Zaks'](https://github.com/mzaks) exceptional Mojo library, [CompactDict](https://github.com/mzaks/compact-dict), which provides blazing fast dictionary implementations. We've incorporated a slightly modified version of this library in the `mojobe.utils` folder (`generic_dict` and `string_dict`); all credits go to him.
- [Gregor Purdy](https://github.com/gnp) has implemented an impressive [Rust port](https://github.com/gnp/minbpe-rs) of `minbpe`. In our initial tests, Gregor's port slightly outperforms our current Mojo port. Remarkable project.

## License

MIT
