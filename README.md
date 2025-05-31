# tree-sitter-twig

Twig grammar for [tree-sitter](https://github.com/tree-sitter/tree-sitter).

## Installation

### npm

```bash
npm install tree-sitter-twig
```

### Cargo

```toml
[dependencies]
tree-sitter-twig = "0.4.0"
```

## Usage

### Node.js

```javascript
const Parser = require('tree-sitter');
const Twig = require('tree-sitter-twig');

const parser = new Parser();
parser.setLanguage(Twig);

const sourceCode = `
{% for item in items %}
  <p>{{ item.name|upper }}</p>
{% endfor %}
`;

const tree = parser.parse(sourceCode);
console.log(tree.rootNode.toString());
```

### Rust

```rust
use tree_sitter::{Language, Parser};

extern "C" { fn tree_sitter_twig() -> Language; }

fn main() {
    let mut parser = Parser::new();
    let language = unsafe { tree_sitter_twig() };
    parser.set_language(language).expect("Error loading Twig grammar");
    
    let source_code = r#"
        {% for item in items %}
          <p>{{ item.name|upper }}</p>
        {% endfor %}
    "#;
    
    let tree = parser.parse(source_code, None).unwrap();
    println!("{}", tree.root_node().to_sexp());
}
```

## Development

### Setup

```bash
npm install
```

### Building

```bash
npm run build        # Generate parser from grammar
npm run build-wasm   # Build WebAssembly version
```

### Testing

```bash
npm test            # Run test suite
```

### Grammar

The grammar is defined in `grammar.js` and supports:

- Template inheritance (`extends`, `block`)
- Control structures (`if`, `for`, `set`, `with`)
- Filters (`variable|filter`) with precedence handling
- Complex expressions with operators and member access
- Array and object literals
- String interpolation (`"text #{expr}"`)
- Named function arguments with dot notation
- Comments and embedded content

## License

Mozilla Public License 2.0
