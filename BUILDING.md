# Building pi-mono

This project supports building with either **npm** or **bun**.

## Quick Start

### Using npm (original)

```bash
# Install dependencies
npm install

# Build all packages
npm run build

# Run from source
npx tsx packages/coding-agent/src/cli.ts --help

# Create binary (outputs as "pi")
npm run build:binary
```

### Using bun

```bash
# Install dependencies
bun install

# Build all packages
bun run build

# Run from source
./pi-bun.sh --help

# Create binary (outputs as "pi-bun")
cd packages/coding-agent && bun run build:binary:bun
```

## Scripts

### Root package.json

| Script | Description |
|--------|-------------|
| `npm run build` | Build all packages using npm |
| `npm run build:bun` | Build all packages using bun |
| `npm run clean` | Clean dist directories |
| `npm run dev` | Run dev mode with watches |
| `npm run test` | Run tests |

### coding-agent package.json

| Script | Description |
|--------|-------------|
| `npm run build` | Build TypeScript (outputs to dist/) |
| `npm run build:binary` | Create standalone binary "pi" (requires bun) |
| `bun run build:binary:bun` | Create standalone binary "pi-bun" |

## Running pi

### npm version
```bash
npx tsx packages/coding-agent/src/cli.ts "your prompt"
```

### bun version
```bash
./pi-bun.sh "your prompt"
```

## Requirements

- Node.js >= 20.0.0
- bun >= 1.0.0 (optional, for bun-based builds)

## Notes

- The `build:binary` and `build:binary:bun` scripts require [bun](https://bun.sh) to compile the standalone executable
- Binary output location: `packages/coding-agent/dist/pi` or `packages/coding-agent/dist/pi-bun`
- Both versions share the same config directory (`.pi`)
