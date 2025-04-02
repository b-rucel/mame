# MAME.js

MAME.js is a TypeScript/JavaScript wrapper for MAME (Multiple Arcade Machine Emulator) that enables running arcade game ROMs directly in web browsers through WebAssembly technology. It provides a clean API for loading and controlling emulated games while bridging the gap between traditional MAME emulation and modern web development practices.


## Architecture

The project consists of several key components:

### Build Environment

The build process is containerized using Docker to ensure consistent compilation:

1. **Dockerfile Configuration**
   - Creates reproducible build environment
   - Installs necessary dependencies (SDL2, Python, Node.js)
   - Configures Emscripten SDK for WebAssembly compilation
   - Enables selective MAME driver compilation

```bash
docker run --rm -v $(pwd)/mame:/mame -w /mame mamejs-compiler:latest /emsdk_portable/emscripten/master/emmake make SUBTARGET="buildname" SOURCES=src/mame/drivers/cps1.cpp,src/mame/drivers/cps2.cpp
 ```

```
1. Purpose The `Dockerfile` creates a containerized build environment for compiling MAME to WebAssembly/JavaScript.

2. Key Components :
- Sets up a Debian environment
- Installs necessary build dependencies (SDL2, Python, Node.js, etc.)
- Installs and configures Emscripten SDK (the tool that compiles C++ to WebAssembly)
- Pre-compiles a test version of MAME (version 0178) with CPS1 drivers

3. Build Process The Dockerfile:
- Creates a consistent build environment
- Handles all dependencies installation
- Provides the Emscripten compiler toolchain
- Allows selective compilation of MAME drivers (like CPS1, CPS2 for specific arcade systems)

4. Integration with MAME.js This container is used to:
- Compile the MAME C++ code into WebAssembly
- Create the JavaScript glue code that MAME.js will later wrap
- Optimize the build for web deployment
```


### Core Components

1. **Emloader**
   - Base emulation loading functionality
   - Handles core emulator initialization
   - Provides interface for controller input

2. **MAME Wrapper**
   - TypeScript classes that wrap the core MAME functionality
   - Handles ROM loading and game execution
   - Maps controller inputs to MAME controls

### Build Process

1. **MAME Compilation**
   - Compiles MAME C++ code to WebAssembly using Emscripten
   - Creates JavaScript glue code for WebAssembly integration
   - Optimizes build for web deployment

### Key Features

- Load and run MAME ROMs in browser through WebAssembly
- Seamless integration with modern web frameworks
- Controller input mapping and handling
- Resolution configuration
- Promise-based async API
- Optimized performance through WebAssembly compilation


## Usage

```typescript
// Basic usage
mamejs.run({
  emulator: "path/to/emulator.js",
  game: {
    files: ["game.zip"],
    driver: "gamedriver"
  },
  resolution: { width: 640, height: 480 }
}, containerElement)
.then(mame => {
  // Game is now running
});

// Direct loader usage
mamejs.load("path/to/emulator.js", containerElement)
.then(mame => {
  return mame.loadRoms(["game.zip"])
    .then(() => mame.runGame("gamedriver"));
});
```


## Controller Support

The system includes built-in controller mappings and can be extended with custom control schemes. Controllers are managed through the `mamejs.controllers` interface.


## Integration

MAME.js is designed to be easily integrated into modern web applications. It handles all the complexity of MAME emulation while providing a clean, Promise-based API for developers. The WebAssembly implementation ensures near-native performance while maintaining broad browser compatibility.


## Dependencies

- Requires a web browser with WebAssembly support
- TypeScript for type definitions and compilation
- MAME core compiled to WebAssembly/asm.js
