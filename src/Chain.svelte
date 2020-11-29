<script>
    import SHA256 from 'crypto-js/sha256'

    class Block {
        constructor(index, timestamp, data, previousHash = '') {
            this.index = index
            this.timestamp = timestamp
            this.data = data
            this.previousHash = previousHash
            this.hash = this.calculateHash()
        }

        calculateHash() {
            return SHA256(
                this.index +
                    this.previousHash +
                    this.timestamp +
                    this.data +
                    this.nonce
            ).toString()
        }
    }

    class Blockchain {
        constructor() {
            this.chain = [this.createGenesis()]
        }

        createGenesis() {
            return new Block(0, 'Genesis block', 'Genesis block', 'Genesis block')
        }

        latestBlock() {
            return this.chain[this.chain.length - 1]
        }

        addBlock(data) {
            let newBlock = new Block()
            newBlock.index = this.latestBlock().index + 1
            newBlock.previousHash = this.latestBlock().hash
            newBlock.timestamp = Date.now()
            newBlock.data = data
            newBlock.hash = newBlock.calculateHash()
            this.chain.push(newBlock)
        }

        checkValid() {
            for (let i = 1; i < this.chain.length; i++) {
                const currentBlock = this.chain[i]
                const previousBlock = this.chain[i - 1]

                if (currentBlock.hash !== currentBlock.calculateHash()) {
                    return false
                }

                if (currentBlock.previousHash !== previousBlock.hash) {
                    return false
                }
            }

            return true
        }
    }

    let jsChain = new Blockchain()
    var counter = 0
    for (var i = 0; i < 1000; i++) {
        jsChain.addBlock(Math.random().toFixed())
        counter += 1
        console.log(counter)
    }
</script>

<style>
    table {
        font-family: 'Trebuchet MS', Arial, Helvetica, sans-serif;
        border-collapse: collapse;
        width: 30vw;
        border: 3px solid #222;
        text-align: left;
        margin-bottom: 10px;
    }
    td,
    th {
        border: 1px solid #ddd;
        padding: 8px;
    }
    tr:nth-child(even){
        background-color: #f2f2f2;
    }
    th {
         padding-top: 12px;
        padding-bottom: 12px;
        background-color: #4CAF50;
        color: white;
    }
</style>

<h1>Is valid: {jsChain.checkValid()}</h1>
{#each jsChain['chain'] as block}
    <table>
        <tr>
            <th>index:</th>
            <td>{block.index}</td>
        </tr>
        <tr>
            <th>timestamp:</th>
            <td>{block.timestamp}</td>
        </tr>
        <tr>
            <th>data:</th>
            <td>{block.data}</td>
        </tr>
        <tr>
            <th>previousHash:</th>
            <td>{block.previousHash}</td>
        </tr>
        <tr>
            <th>hash:</th>
            <td>{block.hash}</td>
        </tr>
    </table>
{/each}
