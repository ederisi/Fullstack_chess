**Full-Stack Bitboard Chess Engine**

A high-performance chess application featuring a custom native C++20 engine and a modern C# Windows Presentation Foundation (WPF) frontend. The project strictly decouples game logic from the user interface, utilizing P/Invoke to bridge the unmanaged backend with the managed .NET 4.7.2 frontend.

**Architecture Overview**
C++ Engine Core: A 64-bit native DLL utilizing bitboards for instantaneous board manipulation, compiled with AVX2 hardware optimizations.

C# WPF UI: A responsive managed layer that handles rendering, user configuration, and asynchronous AI calculations to maintain UI thread responsiveness.

API Boundary: The frontend marshals move data to the unmanaged engine and safely deserializes JSON game state outputs (FEN, status, notation) using System.Text.Json.

**Key Features**
Advanced AI Search: Utilizes a Minimax algorithm with Alpha-Beta pruning and Quiescence search to mitigate the horizon effect in volatile positions.

Optimized Heuristics: Implements Transposition Tables (Zobrist Hashing), Magic Bitboards for sliding pieces, and phase-specific Piece-Square Tables (PSTs).

Game Modes: Supports Human vs. Human and Human vs. Engine gameplay with adjustable AI difficulties (Easy, Medium, Hard).

Dynamic Visuals: Features automatic board flipping based on player perspective and persistent window configurations across sessions.

Strict Memory Management: Safely handles unmanaged memory allocation (IntPtr) and buffer deallocation across the C-style boundary to prevent memory leaks.


**Project structure**
ChessProject/
├── ChessEngine/                             # Native C++20 Backend (DLL)
│   ├── include/                             # C++ Headers
│   │   ├── Bitboard.hpp                     # Board representation and bitwise operations
│   │   ├── BitboardConstants.hpp            # Masks, constants, and bounds
│   │   ├── ChessAI.hpp                      # Minimax, Alpha-Beta, and Quiescence search
│   │   ├── ChessBoard.hpp                   # Game state tracking and FEN generation
│   │   ├── ChessEngineExports.hpp           # C-API boundary for external UI (P/Invoke)
│   │   ├── CustomTypes.hpp                  # Enums, structs, and board state flags
│   │   ├── Magic.hpp                        # Magic bitboard hashing for sliding pieces
│   │   ├── MoveTables.hpp                   # Pre-computed moves for non-sliding pieces
│   │   ├── Moves.hpp                        # Pseudo-legal move generation
│   │   ├── Scoring.hpp                      # Piece-Square Tables (PSTs) and heuristics
│   │   ├── Tables.hpp                       # Pre-computed attack rays and history tables
│   │   ├── Utils.hpp                        # Hardware-optimized bit-scanning utilities
│   │   └── pch.h                            # Precompiled header definition
│   ├── src/                                 # C++ Implementations
│   │   ├── Bitboard.cpp
│   │   ├── ChessAI.cpp
│   │   ├── ChessBoard.cpp
│   │   ├── ChessEngineExports.cpp
│   │   ├── Magic.cpp
│   │   ├── MoveTables.cpp
│   │   ├── Moves.cpp
│   │   ├── Tables.cpp
│   │   ├── dllmain.cpp
│   │   └── pch.cpp
│   ├── ChessEngine.sln                      # Visual Studio Solution
│   ├── ChessEngine.vcxproj                  # MSBuild configuration (AVX2 optimized)
│   └── ChessEngine.vcxproj.filters
│
└── ChessUI/                                 # Managed C# WPF Frontend
    ├── Properties/                          # Application settings and metadata
    │   ├── AssemblyInfo.cs
    │   ├── Resources.Designer.cs
    │   ├── Resources.resx
    │   ├── Settings.Designer.cs
    │   └── Settings.settings                # Persists window dimensions between sessions
    │
    ├── src/                                 # C# Source Code
    │   ├── Assets/Images/                   # UI Assets
    │   ├── Services/                        # Application Logic
    │   │   ├── BoardInteract.cs             # UI click handling and logic coordinate mapping
    │   │   ├── BoardUI.cs                   # Visual grid updates, highlights, and timers
    │   │   ├── ChessEngineInterop.cs        # P/Invoke methods and JSON deserialization
    │   │   ├── ChessGame.cs                 # Mediator connecting the UI to the C++ Engine
    │   │   └── Images.cs                    # Sprite caching dictionary
    │   ├── Views/                           # XAML Layouts & Code-Behind
    │   │   ├── MainWindow.xaml / .cs        # Main chessboard interface and game loop
    │   │   ├── PromotionWindow.xaml / .cs   # Custom pawn promotion popup
    │   │   └── StartWindow.xaml / .cs       # Initial configuration menu (AI depth, timers)
    │   ├── App.config                       # Target framework configuration
    │   └── App.xaml / .cs                   # Application entry point
    ├── packages.config                      # NuGet dependencies (System.Text.Json, etc.)
    ├── app.config
    └── ChessUI.csproj                       # WPF Project Configuration

**Build Instructions**
The solution requires Visual Studio 2022 or newer with the "Desktop development with C++" and ".NET desktop development" workloads installed.

Open the solution environment in Visual Studio 2022.

Set the build configuration to Release and platform to x64 (required for the configured AVX2 instruction sets).

Build the ChessEngine C++ project; a post-build event automatically copies the ChessEngine.dll to the UI's output directory.

Set ChessUI as the startup project and run the application to launch the game interface.

### **Known limitations** *(To be fixed)*
- **Endgame Forced Mates**: The AI currently struggles to detect and execute forced mates in endgame scenarios.

## Contributing
Contributions are welcome! Please fork the repository and submit a pull request with your changes.

## Acknowledgements
- **[Chess Programming Wiki](http://chessprogramming.org/)**: For providing the main insights and theories on chess programming principles
- **[codfish-engine](https://github.com/jsilll/codfish)**: For providing the magic bitboard logic

## Contact
For any questions or suggestions, please open an issue or contact the maintainer directly.
