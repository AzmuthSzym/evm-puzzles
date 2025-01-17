# EVM puzzles

A collection of EVM puzzles. Each puzzle consists on sending a successful transaction to a contract. The bytecode of the contract is provided, and you need to fill the transaction data that won't revert the execution.

## How to play

Clone this repository and install its dependencies (`npm install` or `yarn`). Then run:

```
npx hardhat play
```

And the game will start.

In some puzzles you only need to provide the value that will be sent to the contract, in others the calldata, and in others both values.

You can use [`evm.codes`](https://www.evm.codes/)'s reference and playground to work through this.

My solutions

## Puzzle 1
<code>CALLVALUE</code> pushes given value to stack. <code>JUMP</code> reads the value from the stack in order to get the destination to jump to. In this case <code>JUMPDEST</code> was eight instruction, which was the value we should send.

## Puzzle 2
<code>CODESIZE</code> returns the size of the code currently running (0xA). <code>JUMPDEST</code> is sixth so the answer should be something that 10 - x = 6, which is 4. So, <code>CALLVALUE</code> pushes 4 to stack, <code>CODESIZE</code> pushes 10, <code>SUB</code> does the math and leaves the jump destination on stack so that <code>JUMP</code> can execute properly

## Puzzle 3
<code>CALLDATASIZE</code> gets the size of the input. <code>JUMPDEST</code> is fourth, which means that the size of the data sent should equal to four. It is worth noting that Calldata should be a hexadecimal string with 2 digits per byte.

## Puzzle 4
Very similar to puzle 2. The operation is <code>XOR</code>. <code>CODESIZE</code> is 0xC (0b1100), <code>JUMPDEST</code> is equal to 0xA (0b1010) and is the result of a XOR operation - which looks at differences across bits (its the simplest explanation of XOR in my opinion). The answer is 0x6 (0b0110)

## Puzzle 5
The first three operations execute <code>CALLVALUE</code> squared. Later, this value is comapred using <code>EQ</code> to 0x0100 (256). Only if this comparison is true the jump will happen. 256 = 16^2. 16 -> 0x10, and this is the value we should send

## Puzzle 6
<code>CALLDATALOAD</code> gets input data of current environment. Looking up EVM codes we can see that the data is 32-byte value. <code>JUMPDEST</code> is at 0x0A, so we have to send this position in the correct format: <code>0x000000000000000000000000000000000000000000000000000000000000000A</code>
