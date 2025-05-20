# Generic Board Game Protocol

This protocol is the same as the UCI protocol, except when specified. This document merely describes the differences.
Please refer to both documents.

All commands must end with a UNIX new line character.

*Message* is a synonym to *command*.

A null move is denoted as *none*.

## Notation

### Chess

As in UCI, moves are encoded as long [algebraic notation](https://en.wikipedia.org/wiki/Algebraic_notation_(chess))
and positions as [FEN strings](https://en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation).

### American checkers

Moves and positions are formatted as per the [PDN format](https://en.wikipedia.org/wiki/Portable_Draughts_Notation).

### Nine men's morris

Moves are encoded like so, like in chess algebraic notation:

- place: `a7`
- place + capture: `e4xg7`
- move: `c4-c3`
- move + capture: `d6-f6xa1`

Positions are encoded like in checkers and a bit like in chess, where:

1. The side to move
2. First player pieces separated by comma
3. Second player pieces separated by comma
4. Full move number

Examples:

- initial position: `w:w:b:1`
- after white b4: `b:wb4:b:1`
- after black d2: `w:wb4:bd2:2`
- after white c4: `w:wb4,c4:bd2:2`

## Messages From GUI To Engine

### gbgp

Analogous to the **uci** command from UCI.

### debug (on | off)

### isready

### setoption name `id` [value `x`]

### newgame

Analogous to the **ucinewgame** command from UCI.

This command is mandatory, unlike in UCI.

### position (pos `position` | startpos) [moves `move1` `move2` ...]

Note the keyword *pos*.

### go [searchmoves `move1` `move2` ...] [ponder] [wtime `x`] [btime `x`] [winc `x`] [binc `x`] [movestogo `x`] [depth `x`] [nodes `x`] [movetime `x`]

Note that the *mate* and *infinite* options are discarded.

### stop

### ponderhit

### quit

## Messages From Engine To GUI

### id (name `x` | author `x`)

### gbgpok

Analogous to the **uciok** message from UCI.

### readyok

### bestmove (`move` | none) [ponder `move`]

Note that the null move is denoted as *none*.

### info [depth `x`] [seldepth `x`] [time `x`] [nodes `x`] [pv `move1` `move2` ...] [score (eval `x` [lowerbound | upperbound] | win `x`)] [currmove `move`] [currmovenumber `x`] [hashfull `x`] [nps `x`] [string `str`]

Note the keywords *eval* and *win* from *score*.

Note that *multipv*, *tbhits*, *sbhits*, *cpuload*, *refutation* and *currline* are discarded.

### option name `id` type `t` default `x` [min `x`] [max `x`] [var `x`]

Recommended options are:

- Hash
- Ponder
- OwnBook

The other ones are discarded.
